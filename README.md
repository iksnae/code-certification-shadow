# Agency shadow workspace — iksnae/code-certification

The **brain**, separate from the **body** (the client repo). It holds the agency's
*thinking* — decisions, memory, research, client-profile, retros — so the agency can carry
context across cycles without gating its working state behind human review of shipped code
(ADR-0027).

- **Private + out-of-tree:** this lives in the agency's own storage, OUTSIDE the client's
  repo — the body never tracks the brain, and there is no in-repo path to it.
- **Commits are autonomous; push is operator-gated:** local commits move at the agency's
  speed; pushing publishes un-redacted thinking, so it's a deliberate, approved action.
- **Brain/body boundary:** thinking lives here; what ships goes to the client repo. Crossing
  the line (a shadow proposal → a client PR) is an explicit, provenance-cited promotion.