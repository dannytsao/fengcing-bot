# Test Scenarios

## Bot Flow

- Telegram receives a natural-language query and replies with a processing message.
- Line receives a natural-language query and replies with a processing message.
- The same normalized request produces consistent core judgement across Telegram and Line.
- When the full result is ready, the bot pushes the final response to the original channel.
- When the user exceeds the daily query limit, the bot returns a clear limit message.

## Intent Parsing

Required examples:

- `明晚合歡山拍銀河可以嗎`
- `週六台東海邊拍星空`
- `下週二塔塔加拍夕陽`
- `中部以北`

Expected behavior:

- Extract location, date/time, and task type when the input is specific enough.
- Ask for clarification when location scope is too broad or ambiguous.
- Use Asia/Taipei for all interpreted times.

## Location Resolution

- Resolve specific places through OpenStreetMap.
- Return coordinates and source metadata for clear matches.
- Ask the user to clarify broad regions or vague descriptions.
- Do not fabricate coordinates.

## Data Collection

- CWA data is collected as the first Taiwan weather source.
- Open-Meteo fills complementary data such as dew point and cloud fields.
- Skyfield returns astronomy data for moon and dark-sky related calculations.
- Every data block includes source name, fetch time, and data time when available.
- If any source fails, the result is degraded and the missing source is listed.

## Hermas Orchestration

- Normal path: IntentParser → LocationResolver → DataCollector → PromptBuilder → LLMJudgementService → DecisionPolicy → ResponseFormatter.
- Missing data path: Hermas lowers confidence and applies conservative recommendation.
- Timeout path: Hermas returns a safe failure message and logs the error.
- Ambiguous input path: Hermas asks a clarification question instead of producing a forecast.

## LLM Judgement

- OpenAIProvider receives only system-provided source data.
- LLM output must be parseable JSON.
- LLM must not invent missing values or source names.
- Non-Milky-Way queries must not include the Milky Way section.
- Milky Way queries must include Milky Way Score on a 0-10 scale.

## Response Formatting

Every complete response includes:

- 判讀結論
- 最佳時窗
- 拍攝條件
- 風險與建議
- 依據與來源
- 缺資料警告 when applicable

Milky Way / starry-sky queries additionally include:

- 月光影響
- 大氣透明度
- 結露風險
- 光害備案
- Milky Way Score X/10

## Storage

Google Sheets must record:

- Channel
- User ID
- Query time
- Raw message
- Parsed intent
- Source summary
- Hermas status
- LLM result
- Final response
- Error details when present

Test status values:

- `success`
- `needs_clarification`
- `degraded_missing_data`
- `failed_timeout`
- `failed_exception`
