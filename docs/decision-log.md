# Decision Log

## 2026-05-10

### GitHub project management

Decision: Use GitHub as the single source of truth for requirements, issues, test scenarios, decisions, pull requests, and releases.

Reason: The project will evolve across multiple phases and needs traceable requirement changes, test evidence, and release records.

### Codex maintenance responsibility

Decision: Codex may proactively maintain GitHub records, including issues, docs, templates, labels, milestones, draft PRs, and release note drafts.

Approval gate: Major requirement changes, architecture changes, merge, formal release, and production deployment require user confirmation.

### Storage

Decision: Phase 1 storage uses Google Sheets.

Reason: This matches the astro-bot operating pattern and is fast to inspect during MVP operation.

Future option: Keep a storage interface so PostgreSQL can be added later for membership, billing, analytics, and larger operational needs.

### Hermas Agent

Decision: Include Hermas Agent in the core flow as the orchestrator and final decision policy owner.

Reason: Hermas is useful if it owns data sufficiency checks, fallback behavior, source consistency, confidence adjustment, and final Go/No-Go decisions.

Boundary: Hermas must not be a thin wrapper around services. It owns orchestration and decision policy.

### LLM provider

Decision: Use OpenAI GPT series instead of Claude for Phase 1.

Implementation shape:

- Rename `AIJudgementService` to `LLMJudgementService`.
- Use `OpenAIProvider` for Phase 1.
- Keep the provider interface open for future Claude, Gemini, or local model providers.

Boundary: OpenAI GPT produces professional judgement text and structured JSON from provided data. It does not search sources or invent missing values.

### Data sources

Decision: Phase 1 uses CWA, Open-Meteo, and Skyfield.

Reason: These sources are testable and reproducible. AI search is deferred to Phase 2.

### Geocoding

Decision: Use OpenStreetMap for geocoding.

Ambiguous locations such as `中部以北` or `台東海邊` must trigger a clarification request instead of a guessed representative point.

### Response and scoring

Decision: Milky Way Score uses a 0-10 scale.

Reason: This is easier to read in Telegram and Line messages.

### Deployment

Decision: Deploy MVP to Render.

Reason: Render supports webhook services and simple cloud deployment for the initial bot backend.
