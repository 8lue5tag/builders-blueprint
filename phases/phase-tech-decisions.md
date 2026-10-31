# Phase 2 — Key Technical Decisions

For each applicable decision, present **2-3 options** as a table (Option / What it means / Good for this project because… / Risk-downside), then:

> **My recommendation:** [Option X] because [reason tied to a PRD requirement].

Decisions to cover (skip any that don't apply):
- **Language / framework**
- **Database** — type, specific one, why
- **Hosting / deployment** — where it runs, how you deploy
- **Authentication**
- **External integrations** — how you talk to third parties
- **File / media handling**
- **Background jobs**
- **Caching**
- **Monorepo vs multi-repo**

If the human has strong preferences, respect them as constraints — but flag tradeoffs they may not see.

## Flag the ops deltas

Whenever a decision here **introduces or changes a service, plan/cost, secret, or deploy step**, record it as a **`STACK.md` / `DEPLOYMENT.md` delta** in the build plan (`00.build-<feature>.md`) — same as the Architecture Delta. This is what lets Phase 6 reconcile the ops ledgers instead of letting them silently fall behind.

**Checkpoint:** "All key tech decisions locked in. Agreed?"

> **Residual-risk check:** name the 2-3 decisions here that are hardest to reverse if wrong.
