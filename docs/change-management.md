# Change Management

GitHub is the single source of truth for requirement changes, test scenarios, issue logs, decisions, and releases.

## Change Flow

```text
Requirement change
  ↓
GitHub Issue
  ↓
Impact analysis
  ↓
Update technical design / test scenarios / decision log
  ↓
Draft PR
  ↓
Review and approval when required
  ↓
Merge
  ↓
Release notes
  ↓
Render deployment when approved
```

## Required Records

Every requirement change must include:

- Background and reason
- Impacted modules
- Acceptance criteria
- Test scenarios
- Documentation impact
- Release impact
- Owner and status

## Approval Gates

Codex may proactively create or update:

- Issues
- Documentation
- Templates
- Labels
- Milestones
- Draft PRs
- Release note drafts

User approval is required before:

- Merging major requirement or architecture changes
- Creating a formal release
- Triggering production deployment
- Closing disputed or unverified issues

## Impact Areas

Use these impact areas in issues and PRs:

- Bot Adapter
- ForecastRequestService
- Hermas Agent
- IntentParser
- LocationResolver
- DataCollector
- DecisionPolicy
- LLMJudgementService
- ResponseFormatter
- GoogleSheetsStorage
- Website
- Render Deployment

## Definition of Done

A change is done only when:

- The issue has clear acceptance criteria.
- The implementation or documentation PR links the issue.
- Relevant test scenarios are updated.
- Technical design or decision log is updated when needed.
- Release notes are prepared if the change affects users or operations.
