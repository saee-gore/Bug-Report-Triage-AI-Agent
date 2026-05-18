# Bug-Report-Triage-AI-Agent
• Built a Bug Report Triage Agent deployed via GitHub Actions that automatically classifies severity, applies labels, detects duplicates on every new issue, eliminating repetitive work per report while running at ~$0.005 per issue.

• Reduces manual bug report triage analysis from ~1 hour to ~1 minute — zero infrastructure to maintain.

```
GitHub issue opened → webhook/action → 5 agents run in sequence →
  TriageDecision → optionally apply labels/assignees/comment → done
```
## The five agents

| #  | Agent              | Job                                                    | Model  |
|----|--------------------|--------------------------------------------------------|--------|
| 01 | Severity Classifier | critical / high / medium / low + reasoning            | Haiku  |
| 02 | Issue Classifier    | type, labels (from your taxonomy), keywords, info gaps | Haiku  |
| 03 | Assignee Router     | Keyword overlap with team specialties (no LLM)         | rules  |
| 04 | Duplicate Detector  | TF-IDF cosine vs recent open issues                    | rules  |
| 05 | Comment Drafter     | First-response comment based on the above              | Haiku  |

## Deployment

### As a GitHub Action

Drop `.github/workflows/triage.yml` into your repo and add two secrets:
- `ANTHROPIC_API_KEY`
- `GITHUB_TOKEN` is already provided by GitHub

Runs on every `issues: [opened, reopened]` event. No server needed.
