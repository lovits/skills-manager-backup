# State Artifacts

## Purpose

A harness is easier to trust when its state is visible as concrete artifacts.

## Artifact families

### Session artifacts

- message transcripts
- compacted summaries
- task or todo state
- execution checkpoints

### Memory artifacts

- JSONL histories
- markdown memory files
- profiles, preferences, or long-term notes

### Control-plane artifacts

- heartbeat records
- cron definitions
- runtime status
- lane or session ownership markers

### Governance artifacts

- approval logs
- policy decisions
- tool audit trails

### Recovery artifacts

- resumable plan state
- lock files or work ownership
- failure snapshots

## Questions to force

- Which artifact must exist after a crash?
- Which artifact is user-readable?
- Which artifact is machine-only?
- Which artifact is the source of truth?
