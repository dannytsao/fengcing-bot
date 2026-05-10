# 瘋景人 Bot — Technical Design

**Version**: v1.3  
**Updated**: 2026-05-10  
**Repo**: `dannytsao/fengcing-bot`  
**Deployment**: Render

## 1. Project Overview

瘋景人 Bot 是專為台灣風光攝影愛好者設計的氣象智能判讀系統。使用者透過 Telegram Bot 或 Line Bot 以自然語言提問，後台即時查核多來源氣象與天文資料，產生專業攝影氣象判讀，回傳 Go/No-Go、信心度、最佳時窗、拍攝條件與風險評估。

與 `astro-bot` 的關係：

- `astro-bot` 繼續獨立運行，聚焦天文攝影與天文計算精度。
- 瘋景人 Bot 是新專案，聚焦風光攝影氣象判讀與資料透明度。
- `astro-bot` 的月相、銀河構圖、暗空窗口等天文計算邏輯可移植。

## 2. Design Principles

- Bot-first：主要入口為 Telegram Bot 與 Line Bot，網站為輔助入口。
- 模組化：Adapter / Service / Agent / Core 分層，便於擴充與測試。
- 氣象優先：CWA 官方資料為第一來源，Open-Meteo 補充，Skyfield 處理天文計算。
- 來源透明：每次回覆必須包含來源與時間戳。
- 保守建議：資料不足、來源衝突或系統失敗時採保守 No-Go。
- 可追蹤：需求、測試、issue log、decision log、release 全部在 GitHub 管理。

## 3. Architecture

```text
User Input
  ├── Telegram Bot (TelegramBotAdapter)
  ├── Line Bot (LineBotAdapter)
  └── Website (service entry only)
          ↓
ForecastRequestService
          ↓
HermasAgent (orchestrator and decision authority)
  ├── IntentParser
  ├── LocationResolver (OpenStreetMap)
  ├── DataCollector
  │     ├── CWA API
  │     ├── Open-Meteo
  │     └── Skyfield
  ├── DecisionPolicy
  ├── PromptBuilder
  ├── LLMJudgementService
  │     └── OpenAIProvider
  └── ResponseFormatter
          ↓
GoogleSheetsStorage
```

## 4. Key Decisions

- Backend framework: Python FastAPI.
- Deployment: Render.
- Bot channels: Phase 1 includes both Telegram and Line.
- Agent layer: Hermas Agent is included in the core path.
- Hermas role: Orchestrator and final decision policy owner.
- LLM provider: OpenAI GPT series via `OpenAIProvider`.
- Data sources: CWA, Open-Meteo, Skyfield.
- AI search: Deferred to Phase 2.
- Geocoding: OpenStreetMap.
- Ambiguous locations: Ask the user to clarify; do not guess a representative point.
- Storage: Google Sheets for Phase 1.
- Reply flow: First reply with processing status, then push the full result.
- Milky Way Score: 0-10 scale.

## 5. Module Responsibilities

### Bot Adapters

`TelegramBotAdapter` and `LineBotAdapter` receive webhooks, normalize channel/user/message/timestamp, send an initial processing reply, and later push the final response.

### ForecastRequestService

Responsible for channel-neutral request handling, rate limits, deduplication, error capture, and storage writes. It delegates forecast judgement to Hermas Agent.

### HermasAgent

Responsible for orchestration and final decision policy. Hermas coordinates parsing, location resolution, data collection, LLM judgement, response formatting, and conservative fallback behavior.

### IntentParser

Extracts location, date/time, and task type from natural language. Supported task types include Milky Way, starry sky, sunset glow, and general landscape photography.

### LocationResolver

Uses OpenStreetMap geocoding for clear locations. For broad or vague regions such as `中部以北` or `台東海邊`, it asks the user for a more precise location.

### DataCollector

Collects source data from CWA, Open-Meteo, and Skyfield. Each collected data item must include source metadata and timestamps.

### LLMJudgementService

Uses OpenAI GPT series to produce professional judgement and structured JSON from system-provided data. The model must not invent sources or values.

### DecisionPolicy

Owned by Hermas. It determines final Go/No-Go, confidence, degradation behavior, and missing-data warnings.

### ResponseFormatter

Formats validated results into Traditional Chinese messages suitable for Telegram and Line.

### GoogleSheetsStorage

Records channel, user ID, query time, raw message, parsed intent, source summary, Hermas status, LLM result, final response, and errors.

## 6. Response Format

Every full response must include:

```text
【判讀結論】
Go / No-Go  信心度：XX%

【最佳時窗】
HH:MM ～ HH:MM（Asia/Taipei）

【拍攝條件】
- 雲量：XX%
- 降雨機率：XX%
- 風速：X m/s（陣風 X m/s）
- 濕度：XX%
- 露點差：X°C（結露風險：低/中/高）
- 能見度：XX km

【銀河星空】（銀河或星空查詢時才出現）
- 月光影響：...
- 大氣透明度：Good/Fair/Poor
- 結露風險：低/中/高
- 光害備案：...
- Milky Way Score：X/10

【風險與建議】
...

【依據與來源】
- CWA：查核時間 HH:MM，資料時間 HH:MM
- Open-Meteo：查核時間 HH:MM
- Skyfield：天文計算

【缺資料警告】
...
```

## 7. Phase Plan

### Phase 1 MVP

- Create modular FastAPI project structure.
- Implement Telegram and Line webhook adapters.
- Integrate Hermas Agent as Python module/package.
- Implement IntentParser and OpenStreetMap LocationResolver.
- Integrate CWA, Open-Meteo, and Skyfield.
- Implement OpenAI-backed LLMJudgementService.
- Implement DecisionPolicy and ResponseFormatter.
- Log to Google Sheets.
- Deploy to Render.

### Phase 2

- Add AI search as supplemental source only.
- Add website service entry page.
- Improve rate limiting and operational dashboards.

### Phase 3+

- Add microclimate correction, forecast-vs-actual tracking, Clear Outside/Meteoblue integration, personalization, subscription, and paid membership features.
