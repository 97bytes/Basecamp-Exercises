**Client & pilot**
Meridian — an AI agent that triages and resolves customer support tickets (a coordinator routing to billing / technical / account specialists). Live 3 weeks; closing tickets that aren't actually resolved.

**What's actually breaking it**
The coordinator's instructions forced it to pick exactly one specialist per ticket, even when a ticket raised multiple problems. It would handle one issue (e.g. SSO), then fabricate a resolution for the rest without actually delegating them. The customer got a confident reply; the problem stayed open.

**The fix**
One instruction change in `system-prompt-coordinator.txt`, step 5: removed the "one specialist only" rule and replaced it with "spawn one specialist per distinct category the ticket touches." No code changes. No model changes.

**Proof**
- Baseline (Sonnet): RESOLVED 0/5 — $0.12/trial
- After fix (Sonnet): RESOLVED 5/5 — $0.15/trial
- Holdout tickets (3 tickets, 9 trials): RESOLVED 9/9 — $0.15/trial

**What it would take**
The fix is already live — it's a prompt edit, already validated. Next step: run a broader regression over the full ticket backlog to confirm no regressions on single-issue tickets. Estimated 1–2 days. Cost stays within current budget at $0.15/trial.

**The objection we'll get**
"Why not just use a better model?" — we tested it. Claude Opus (the next tier up) scored 0/5 on the same ticket at $0.25/trial. Same failure, 70% more expensive. The model was never the problem. The instructions were.

