# The NorthStar Skill Foundry

**Claude Code skills that interview your team before writing a line.**

Most public Claude Code skills are generic. They know nothing about your dbt models, your naming conventions, your review process, or the Slack channel where analysts report broken charts. So they produce generic output, and your analysts quietly stop using them.

These skills are different. They are **skill builders**: each one inspects your repository, asks your team a short set of questions — the questions a consultant asks in the first week of an engagement — and then writes a custom skill calibrated to *your* stack. They are tool-agnostic by design: the builder detects what it can, asks about your source and target tools and what access Claude Code has (MCP servers, APIs, exports), and grows the skill from the answers. The output is versioned in your repo and maintained like any other code.

They exist because I built the originals inside a global fintech, during one of the largest Looker → Lightdash migrations in Europe: 10,000+ dashboards audited, 2,000+ migrated, 700+ analysts trained across 15+ countries. The skills those analysts ran daily — `/fix-chart`, `/fix-explore`, `/migrate-look` — started exactly like this: with questions, not code.

## The builders

| Builder | Generates | Born from |
|---|---|---|
| [`build-fix-chart`](skills/build-fix-chart/SKILL.md) | `/fix-chart` and `/fix-explore` — diagnose a broken chart down to the dbt model and open the PR | The morning we opened 13 PRs across 8 dbt repos and fixed 19 explores before noon |
| [`build-migrate-chart`](skills/build-migrate-chart/SKILL.md) | `/migrate-chart` — convert reports between any BI pair (Looker→Lightdash, Tableau→Power BI, …), matching your conventions and using whatever access you have (MCP, API, exports) | 2,000+ dashboards migrated; the fanout bugs, the mid-migration incident, all of it |
| [`build-dashboard-triage`](skills/build-dashboard-triage/SKILL.md) | `/dashboard-triage` — KEEP / MIGRATE / ARCHIVE verdicts with evidence | The audit that found 7,000 orphaned dashboards. Archived with a 30-day reclaim window. Zero reclaims. |
| [`build-dbt-model`](skills/build-dbt-model/SKILL.md) | `/new-model` — scaffold dbt models indistinguishable from your best existing ones, after checking one doesn't already exist | A BI tool exposing 300 tables nobody could navigate. We got it to 50, and self-serve started working. |
| [`build-settle-the-number`](skills/build-settle-the-number/SKILL.md) | `/settle-the-number` — trace two conflicting numbers to the exact line where their definitions diverge, verdict in language a CFO accepts | Every boardroom where two dashboards disagreed and the meeting stopped |
| [`build-publish-check`](skills/build-publish-check/SKILL.md) | `/publish-check` — a governance gate a dashboard must pass before it counts as "published" | A 200-dashboard estate reduced to 32 governed reports. Self-serve started working the same month. |
| [`build-training-programme`](skills/build-training-programme/SKILL.md) | A session-by-session curriculum built on your real backlog, plus `/coach` — a practice skill for the gaps between sessions | 700+ analysts trained across 15+ countries; ~90 minutes to a first shipped chart |
| [`build-knowledge-base`](skills/build-knowledge-base/SKILL.md) | A demand-driven knowledge base structure, plus `/write-doc` — documentation welded to the code, with a staleness check | A 54-page knowledge base and 100+ pages of playbooks that analysts actually opened |
| [`build-support-triage`](skills/build-support-triage/SKILL.md) | `/support-triage` — first responder for your analytics support channel: classify, diagnose, route, escalate | 10–30 support messages a day, personally answered, routed and escalated — for months |
| [`build-board-pack`](skills/build-board-pack/SKILL.md) | `/board-pack` — assemble the recurring board/trade pack from governed metrics only, commentary in your house style | The monthly trade deck that became the cornerstone of strategic planning at a payments scale-up |
| [`build-dashboard-health`](skills/build-dashboard-health/SKILL.md) | `/health-check` — the ongoing lifecycle loop: drift detection, Draft/Verified/Drifted/Archived transitions, badge automation | The post-migration governance framework: every dashboard with an owner and a lifecycle stage |

## How a builder works

Every builder follows the same three-phase protocol:

1. **Inspect.** It reads your repository first — dbt `dbt_project.yml`, Lightdash/Looker config, model naming patterns, CI setup. It never asks a question the code can already answer.
2. **Interview.** It asks 5–8 questions. Not "what is your BI tool" — it already knows. Questions like *"When a chart breaks, which Slack channel hears about it first?"* and *"Who is allowed to merge to the dbt repo that feeds Finance dashboards?"* The questions encode ten years of scar tissue.
3. **Generate.** It writes a complete `SKILL.md` into your `.claude/skills/`, encoding your answers as hard rules — your naming conventions, your review gates, your escalation paths. The generated skill passes your code review because it was built from your standards.

## They work as a system

The generated skills know about each other. `/support-triage` delegates a broken-chart message to `/fix-chart` and a "numbers don't match" message to `/settle-the-number`. `/publish-check` gates the estate's entrance, `/health-check` runs the lifecycle loop, `/dashboard-triage` makes the big one-off calls. `/write-doc` picks up the questions `/support-triage` sees eleven times. Install one and it works alone; install several and the support load starts going down instead of merely being absorbed.

## Installation

```bash
git clone https://github.com/NorthStar-Analytics-LTD/skill-foundry
cp -r skill-foundry/skills/* your-repo/.claude/skills/
```

Then, inside Claude Code, run any builder:

```
/build-fix-chart
```

## What these are not

These builders produce a competent starting point — the skill your team should have had on day one. They do not replace the engagement work: shadowing analysts to find where the hours actually go, training every analyst until they ship something real, and the train-the-trainer programme that keeps adoption spreading after I leave.

If the generated skill makes your team faster, imagine the version built after two weeks inside your workflows.

**NorthStar Analytics** — [northstaranalytics.co.uk](https://northstaranalytics.co.uk) · AI Enablement for Analytics Teams

---

*British English throughout. No buzzwords were harmed in the making of these skills.*
