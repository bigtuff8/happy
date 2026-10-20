<!-- gate:contract
SECTION: What you're being asked to approve
  There is no proposal to approve here. This run arrived with no ticket and no stated
  intent, so there was nothing concrete for Discovery to investigate. This document
  surfaces that gap and asks you to choose how to proceed, rather than presenting a
  scoped plan that would have been invented rather than discovered.

SECTION: Plain-English summary
  The target, bigtuff8/happy, is a plain GitHub fork of the open-source "Happy Coder"
  project (slopus/happy). It has no changes of its own: its main branch currently sits
  1,359 commits behind the upstream project it was copied from, and its last
  "upstream catch-up" automation ran in August 2026. Issues are disabled on the fork,
  so there is no backlog to read either. With no ticket, no operator instruction, and
  no fork-specific history to investigate, Discovery has nothing to turn into a
  curatable requirements suite, a CX brief, or a phased plan — doing so anyway would
  mean inventing the brief, which this role exists to avoid.

SECTION: Behaviour change the operator needs to know about
  None. No code, design or build work has happened as part of this run. Nothing about
  the running app or the fork has changed.

SECTION: Anything unverified
  No MCP servers (including Jira) are connected to this session, so "Epic: none"
  below reflects the absence of a ticket to look up, not a failed Jira lookup. Whether
  bigtuff8/happy is meant to track upstream passively, host independent development,
  or serve only as a harness test target is unverified — inferred from public GitHub
  metadata (fork flag, disabled issues, merged "upstream catch-up" PRs) rather than
  confirmed with anyone who owns it.

DECISION: How should this run proceed given no ticket was attached? | Supply a specific ticket or backlog item for Discovery to investigate (recommended) | Treat "catch the fork up to upstream" itself as the ticket and let Discovery scope that sync | Close this run with no further action
-->

# Discovery: bigtuff8/happy — run 2ad79503, attempt 1

Epic: none

## Blocker (read first)

**No ticket and no operator intent were attached to this run.** `HARNESS_TICKET` is empty
and the dispatch brief states "Operator intent: (none given)". Discovery's whole job is to
turn a stated problem into a traceable proposal; with no problem stated, every one of the
required outputs (curatable requirements suite, CX success criteria, data scope, test
scope, phased plan) would have to be invented rather than discovered. Per this run's own
invariant — *"a gap in the signed-off input is surfaced, not improvised around"* — that gap
is reported here instead of papered over with a self-assigned feature.

This blocker needs the operator (or whoever dispatches the next attempt) to act: attach a
ticket, or confirm one of the options in the decision above. Owner: operator.

## What was verified about the current state

All items below were checked directly in this worktree and against GitHub; none are
carried over from memory or assumption.

| Item | Verified finding | Source |
|---|---|---|
| Target repo | `bigtuff8/happy` — Mobile/Web client for Claude Code & Codex ("Happy Coder") | `README.md`; `git remote -v` |
| Fork relationship | Confirmed fork of `slopus/happy` | `gh repo view bigtuff8/happy --json isFork,parent` |
| Divergence from upstream | `main` has 0 commits that `upstream/main` lacks; `upstream/main` has 1,359 commits that `main` lacks | `git rev-list --left-right --count main...upstream/main` (after `git fetch upstream`) |
| Local/fork-specific changes | None found — the fork's own commit log is identical to a past point in upstream's history, not a divergent line | `git log --oneline upstream/main..HEAD` → empty |
| Fork's own change process | An automated "upstream catch-up" workflow exists and has run before (merged PRs #1–#4: two "upstream catch-up" PRs and two "CLEAN auto-apply" PRs), most recently 2026-08-31 | `gh pr list -R bigtuff8/happy --state all` |
| Backlog on the fork | GitHub Issues are disabled on `bigtuff8/happy` | `gh issue list -R bigtuff8/happy` → "repository has disabled issues" |
| Ticket system | No ticket ID was supplied (`HARNESS_TICKET` empty); no MCP/Jira connection is live this session | Session environment; "MCP servers on: none" in dispatch brief |
| Upstream project backlog (context only, not this fork's) | `docs/roadmap.md` in the tree lists upstream's own open wishlist (navigation bugs, workspaces/checkouts, push-notification routing, etc.) — this is slopus/happy's roadmap, carried into this fork's tree, not anything scoped to bigtuff8 | `docs/roadmap.md` |

## Anti-reinvention check

Not applicable in the usual sense: there is no proposed integration, service, or mechanism
to check for duplication against, because no feature has been scoped. The one adjacent
mechanism worth naming is the fork's existing "upstream catch-up" automation (see table
above) — if the eventual ticket turns out to be "keep the fork current," that mechanism
already exists and should be re-run or repaired, not replaced.

## Mandatory evaluation criteria applied to the options below

1. **Token/cost efficiency.** All three options in the decision are process choices, not
   systems to build, so none carries a recurring LLM or infrastructure cost by itself.
   Option 2 (scope a sync) would, in a later Discovery pass, need to cost out whether the
   sync is a single rebase/push (near-zero cost, see `AGENTS.md`'s "Sync To Main" recipe)
   or a merge requiring conflict resolution across 1,359 commits (materially higher,
   bounded by the size of the diff, not open-ended).
2. **Silent background running.** None of the three options introduces any new
   daemon, watcher, poller, or scheduled task. The fork's existing "upstream catch-up"
   automation (noted above) is itself a background mechanism already in place outside
   this run; this Discovery pass did not inspect its schedule or visibility and flags
   that as unverified rather than assuming it is silent.

## Options (for the operator decision above)

| Option | What it unlocks | Outcome traced to | Cost/background note |
|---|---|---|---|
| Supply a specific ticket or backlog item (recommended) | Lets Discovery run its normal process (investigate, map current vs. target, define CX criteria, phase the work) against a real business outcome | Every required Discovery output depends on this | No cost until scoped; this is the default, no-compromise path |
| Treat "catch the fork up to upstream" as the ticket | A concrete, boundable piece of work (close the 1,359-commit gap) that is already partly automated | Keeps the fork a true mirror, which is the only reason a fork-without-divergence exists | Low token cost if a clean rebase is possible; cost rises with conflict count — must be bounded before starting, per criterion 1 above |
| Close this run with no further action | Avoids inventing scope | N/A — no outcome, because there is no stated need | Zero cost, zero background impact — but produces nothing |

## Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Next attempt re-dispatches with the same empty ticket/intent, repeating this outcome | Medium | Low (time only, no harm) | Operator attaches a ticket or intent before re-dispatch |
| Someone scopes a feature from `docs/roadmap.md` without confirming it's the intended target (that file is upstream's wishlist, not a bigtuff8-specific commitment) | Medium | Medium — could build the wrong thing | Treat roadmap items as candidates only, never as a substitute for operator intent |

## High-level next steps

1. Operator resolves the decision above — most likely by attaching a specific ticket or
   confirming the fork-sync framing — before any further Discovery, Design, or Build
   dispatch for this run.
2. If a ticket is attached: re-run Discovery in full against it; everything in this
   document (fork state, lack of divergence, disabled issues, existing catch-up
   automation) remains valid verified context and does not need re-checking.
3. If "catch up the fork" is confirmed as the task: the next Discovery pass should size
   the 1,359-commit gap (conflict count, touched areas) before costing it, per the
   token-efficiency criterion above, rather than assuming a clean rebase.

No phase of build work is recommended from this run; there is nothing yet to build.
