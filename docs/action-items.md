# Action Items

## Record Date: 2026-05-11

These are the next planned action items for Fengcing Bot. Execute in sequence unless a new requirement change is approved and recorded.

| Seq | Action Item | Expected Output | Status |
|---:|---|---|---|
| 1 | Establish Phase 1 GitHub tracking structure | `Phase 1 MVP` milestone, project board, and core labels | Pending |
| 2 | Break Phase 1 into implementation issues | GitHub issues for each MVP workstream | Pending |
| 3 | Confirm external accounts and secrets | Confirm availability of CWA API key, Telegram token, Line credentials, OpenAI API key, Google Sheets service account, and Render setup | Pending |
| 4 | Define environment variables | Add `docs/deployment-env.md` with variable names, purpose, required/optional status, and setup notes | Pending |
| 5 | Create implementation branch and project skeleton | `feat/phase-1-foundation`, FastAPI app structure, config, logging, health check, and test framework | Pending |
| 6 | Build the first end-to-end vertical slice | Telegram webhook receives message, replies processing, Hermas stub returns fixed result, storage/logging stub records request, Render deploy path verified | Pending |
| 7 | Replace stubs incrementally | Implement IntentParser, LocationResolver, DataCollector, OpenAIProvider, DecisionPolicy, ResponseFormatter, and GoogleSheetsStorage | Pending |
| 8 | Add tests and acceptance verification | Verify core scenarios: `明晚合歡山拍銀河可以嗎`, `週六台東海邊拍星空`, `下週二塔塔加拍夕陽`, `中部以北` | Pending |
| 9 | Prepare Phase 1 MVP release candidate | Release checklist issue, updated test scenarios, `v0.1.0` release notes draft, deployment approval gate | Pending |

## Next Session Starting Point

Start with action items 1 and 2:

1. Create GitHub milestone, labels, and project board for Phase 1 MVP.
2. Create implementation issues for each Phase 1 workstream.

## Governance Notes

- Any change to this sequence must be recorded as a requirement or decision update.
- Major merge, formal release, and production deployment still require user approval.
- Keep this file updated when action items are completed, deferred, or changed.
