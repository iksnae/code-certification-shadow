# Client profile — iksnae/code-certification

Durable facts about this engagement. Read by every role to ground its work; maintained by the
account manager. Internal — never shipped to the client.

## Who
- Client / engagement: iksnae/code-certification
- Account manager: Mara

## Product
**Certify** — a Go CLI ("code trust, with an expiration date"). It continuously evaluates
every code unit in a repository, scores it across 9 weighted quality dimensions (correctness,
maintainability, readability, testability, security, architectural fitness, operational
quality, performance, change risk), and issues **time-bound certifications** (default 90-day
window, risk-adjusted) that expire by design. The pitch vs. CI: CI says whether code passes
right now; Certify says whether code should still be trusted.

Core capabilities we build against:
- **Certification pipeline** — `certify init → scan → certify → report` (bootstrap, discover
  units, collect evidence + evaluate/score, emit report card + shields.io badge).
- **Report formats** — the surface we most often touch: `card` (terminal), `full` (per-unit
  markdown), `json` (machine-readable), `text` (brief health summary), plus `--site` (a
  self-contained offline HTML report). Report code lives in `internal/report/`
  (card.go / detailed.go / full.go / health.go / site.go).
- **Polyglot analysis tiers** — Go (native `go/ast`, deepest), TypeScript/Python/Rust
  (tree-sitter + linters), and a **generic file-level fallback for every other language**.
  Certification records and grades live in `internal/domain/`.

Our recent work has centered on the **unsupported-language seam**: languages with no analysis
tier were getting hollow, fabricated grades (Issue #21 — Swift repos scored a flat Grade C
regardless of quality). We delivered PR #33 (Issue #27) — an unsupported-units banner across
all report formats plus exclusion of unsupported units from aggregate grade math. Issue #25
(guarding `Score()`/`scoreFromMetrics` for unsupported languages, adding N/A + Unsupported to
the Grade/record types) is investigated and change-planned but not yet delivered.

_Sources: client repo README.md, docs/VISION.md; report/domain layout from
internal/report/ + internal/domain/; ENGAGEMENT-HISTORY.md (#21/#25/#27), closeouts/27.md._

## Stack & constraints
- Language / module: **Go 1.25.5**, module `github.com/iksnae/code-certification`
  (source: `go.mod`). Public install docs state Go 1.22+ / Git as the floor.
- Test: `go test ./... -count=1`
- Build: `go build -o build/bin/certify ./cmd/certify/`
- Model: `deepseek/deepseek-v4-pro` (provider kind `deepseek`), verification timeout 300000 ms.
- Key deps: cobra (CLI), smacker/go-tree-sitter, golang.org/x/tools, yaml.v3.
- Constraints to respect: report features are **cross-cutting** — a change to grade logic or a
  banner touches all four report pipelines (card/detailed/full/health) and their test files at
  once (per closeout #27). Site generation has a stated **performance expectation** (≈559 units
  → 584 pages in under 2s; works offline via `file://`, no external deps). Test fixtures for
  new unsupported languages follow the `testdata/unsupported/` directory convention.

_Sources: go.mod, .agencyx/config.json (verification block + provider), closeouts/27.md,
client README.md (perf/offline claims, Go floor)._

## Voice
**Not yet established.** No client communication preferences are recorded in the shadow (README,
MEMORY, DECISIONS, ENGAGEMENT-HISTORY, or closeouts) — the relationship has run as
issue-driven delivery (#21/#25/#27) with no stated tone preference from the client. Default to
**concise, technical, evidence-cited updates**: lead with the outcome, name the PR/issue, and
state plainly what was and wasn't verified. Revisit and record here the first time the client
signals a preference.

_Source: absence across all shadow docs — stated as unknown rather than invented._