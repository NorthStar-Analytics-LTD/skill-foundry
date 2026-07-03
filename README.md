# The NorthStar Skill Foundry

**Claude Code skills that interview your team before writing a line.**

Most Claude Code skills are generic. They know nothing about your dbt models, your naming conventions, your review process, or the Slack channel where analysts report broken charts. So they produce generic output, and your analysts quietly stop using them.

These skills are different. They are **skill builders**: each one inspects your repository, asks your team a short set of questions — the questions a consultant asks in the first week of an engagement — and then writes a custom skill calibrated to *your* stack. They are tool-agnostic by design: the builder detects what it can, asks about your source and target tools and what access Claude Code has (MCP servers, APIs, exports), and grows the skill from the answers. The output is versioned in your repo and maintained like any other code.

They exist because I have spent a decade embedded in analytics teams — running BI migrations, auditing dashboard estates, training 700+ analysts across 15+ countries, and answering the support channel myself. Every builder encodes a lesson that cost something to learn. The original skills my clients' analysts run daily started exactly like this: with questions, not code.

## The builders

| Builder | Generates | The lesson it encodes |
|---|---|---|
| [`build-fix-chart`](skills/build-fix-chart/SKILL.md) | `/fix-chart` and `/fix-explore` — diagnose a broken chart down to the dbt model and open the PR | When a chart breaks, the dashboard is usually innocent — the problem is further down |
| [`build-migrate-chart`](skills/build-migrate-chart/SKILL.md) | `/migrate-chart` — convert reports between any BI pair (Looker→Lightdash, Tableau→Power BI, …), matching your conventions and using whatever access you have (MCP, API, exports) | Tool-side SQL, fanout and parallel editing are where migrations die |
| [`build-dashboard-triage`](skills/build-dashboard-triage/SKILL.md) | `/dashboard-triage` — KEEP / MIGRATE / ARCHIVE verdicts with evidence | Nobody misses dashboards they can't name |
| [`build-dbt-model`](skills/build-dbt-model/SKILL.md) | `/new-model` — scaffold dbt models indistinguishable from your best existing ones, after checking one doesn't already exist | The cheapest model is the one you don't build |
| [`build-settle-the-number`](skills/build-settle-the-number/SKILL.md) | `/settle-the-number` — trace two conflicting numbers to the exact line where their definitions diverge, verdict in language a CFO accepts | Two numbers that disagree are answering two different questions |
| [`build-publish-check`](skills/build-publish-check/SKILL.md) | `/publish-check` — a governance gate a dashboard must pass before it counts as "published" | Every clean-up regrows unless a gate holds the boundary |
| [`build-training-programme`](skills/build-training-programme/SKILL.md) | A session-by-session curriculum built on your real backlog, plus `/coach` — a practice skill for the gaps between sessions | Analysts learn by shipping, not by watching |
| [`build-knowledge-base`](skills/build-knowledge-base/SKILL.md) | A demand-driven knowledge base structure, plus `/write-doc` — documentation welded to the code, with a staleness check | A doc that has drifted from the code answers confidently and wrongly |
| [`build-support-triage`](skills/build-support-triage/SKILL.md) | `/support-triage` — first responder for your analytics support channel: classify, diagnose, route, escalate | Most support messages are one of five shapes, and the shape determines the route |
| [`build-board-pack`](skills/build-board-pack/SKILL.md) | `/board-pack` — assemble the recurring board/trade pack from governed metrics only, commentary in your house style | No number enters the pack unless it traces to a governed definition |
| [`build-dashboard-health`](skills/build-dashboard-health/SKILL.md) | `/health-check` — the ongoing lifecycle loop: drift detection, Draft/Verified/Drifted/Archived transitions, badge automation | The audit is an event; rot is a process |

## How a builder works

Every builder follows the same four-phase protocol:

1. **Inspect.** It reads your repository first — dbt `dbt_project.yml`, BI tool artefacts, model naming patterns, CI setup, existing MCP and API access. It never asks a question the code can already answer.
2. **Interview.** It asks 5–8 questions. Not "what is your BI tool" — it already knows. Questions like *"When a chart breaks, which Slack channel hears about it first?"* and *"Who is allowed to merge to the dbt repo that feeds Finance dashboards?"* The questions encode ten years of scar tissue.
3. **Generate.** It writes a complete `SKILL.md` into your `.claude/skills/`, encoding your answers as hard rules — your naming conventions, your review gates, your escalation paths. The generated skill passes your code review because it was built from your standards.
4. **Learn.** Every generated skill ships with a fourth phase built in: when it hits a case its rules do not cover, it stops and asks a human instead of guessing — then proposes a PR to its own SKILL.md encoding what it just learned. This is how a fifty-dashboard clean-up actually happens: a simple skill fixes the first ten, learns from what analysts correct on the next twenty, and by dashboard fifty it is catching cases the first version never could. Every analyst who runs it becomes, briefly, someone teaching it.

## They work as a system

The generated skills know about each other. `/support-triage` delegates a broken-chart message to `/fix-chart` and a "numbers don't match" message to `/settle-the-number`. `/publish-check` gates the estate's entrance, `/health-check` runs the lifecycle loop, `/dashboard-triage` makes the big one-off calls. `/write-doc` picks up the questions `/support-triage` sees eleven times. Install one and it works alone; install several and the support load starts going down instead of merely being absorbed.

## Installation

Download the builders from [northstaranalytics.co.uk/skill-foundry](https://northstaranalytics.co.uk/skill-foundry) and unzip into your repo:

```bash
unzip skill-foundry.zip -d your-repo/.claude/skills/
```

Or pull a single builder:

```bash
mkdir -p .claude/skills/build-fix-chart
curl -fsSL https://northstaranalytics.co.uk/skills/build-fix-chart.md \
  -o .claude/skills/build-fix-chart/SKILL.md
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
