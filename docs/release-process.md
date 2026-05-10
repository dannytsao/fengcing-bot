# Release Process

## Release Principles

- Every production release must have a GitHub Release record.
- Every release must reference the merged PRs or issues included.
- Major requirement, architecture, and production deployment actions require user approval.
- Render deployment must be treated as a release gate, not an invisible side effect.

## Release Checklist

Before release:

- [ ] All included issues have acceptance criteria.
- [ ] PRs are merged into `main`.
- [ ] Test scenarios are updated.
- [ ] Technical design is updated when behavior or architecture changed.
- [ ] Decision log is updated when a major decision changed.
- [ ] Bot flow has been verified for Telegram and Line when affected.
- [ ] Google Sheets logging has been verified when affected.
- [ ] Environment variables are documented when changed.
- [ ] Render deployment approval has been received.

After release:

- [ ] GitHub Release notes are published.
- [ ] Related issues are closed or moved to follow-up.
- [ ] Project board status is updated.
- [ ] Any known limitations are recorded.

## Versioning

Use lightweight semantic versioning after MVP starts shipping:

- `v0.x` for MVP and pre-production releases.
- `v1.0` for first production-ready release.
- Patch versions for bug fixes.
- Minor versions for new capabilities.

## Release Notes Template

```md
# Release vX.Y.Z

## Summary

## Changes

## Test Evidence

## Known Limitations

## Deployment

## Follow-ups
```

## Deployment Gate

A deployment to Render may proceed only after:

- Release PR is merged.
- Required secrets are configured.
- User confirms production deployment.
- Rollback path is known.
