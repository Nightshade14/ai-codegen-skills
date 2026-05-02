# ai-codegen-skills

A curated set of agent skills for real engineering work — not vibe coding.

Developing real applications is hard. Approaches like GSD, BMAD, and Spec-Kit try to help by owning the process. But while doing so, they take away your control and make bugs in the process hard to resolve.

These skills are designed to be small, easy to adapt, and composable. They work with any model and are grounded in software engineering fundamentals. Hack around with them. Make them your own. Enjoy.

## Install

### Codex

Add the repo marketplace, restart Codex, then install the plugin from the plugin directory:

```shell
codex plugin marketplace add Nightshade14/ai-codegen-skills --sparse .agents/plugins
```

After restarting Codex, open the plugin directory, choose `Nightshade14 Repo Plugins`, and install `AI Codegen Skills`.

Codex packaging lives in `.codex-plugin/plugin.json`, and Codex marketplace discovery for this repo lives in `.agents/plugins/marketplace.json`.

### Claude Code Marketplace

Add the marketplace and install the plugin directly from within Claude Code:

```shell
/plugin marketplace add Nightshade14/ai-codegen-skills
/plugin install skills@nightshade14-ai-codegen-skills
```

Skills are namespaced under the plugin: `/skills:tdd`, `/skills:diagnose`, etc.

Claude packaging remains under `.claude-plugin/`.

### Claude Code Local Setup

Clone the repo and symlink all skills into `~/.claude/skills/`:

```bash
git clone git@github-personal:Nightshade14/ai-codegen-skills.git
cd ai-codegen-skills
./scripts/link-skills.sh
```

Then run `/setup-skills` once per repo you want to use the engineering skills in.

## Why These Skills Exist

These skills exist to fix common failure modes with Claude Code, Codex, and other coding agents.

### #1: The Agent Didn't Do What I Want

> "No-one knows exactly what they want"
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**The Problem**. The most common failure mode in software development is misalignment. You think the dev knows what you want. Then you see what they've built — and you realize it didn't understand you at all.

This is just the same in the AI age. There is a communication gap between you and the agent. The fix is a **grilling session** — getting the agent to ask you detailed questions about what you're building.

**The Fix** is to use:

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md) — for non-code uses
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) — same as `/grill-me`, but also sharpens your domain language and updates ADRs

These help you align with the agent before you get started, and think deeply about the change you're making. Use them _every_ time you want to make a change.

### #2: The Agent Is Way Too Verbose

> With a ubiquitous language, conversations among developers and expressions of the code are all derived from the same domain model.
>
> Eric Evans, [Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**The Problem**: Agents are usually dropped into a project and asked to figure out the jargon as they go. So they use 20 words where 1 will do.

**The Fix** is a shared language document (`CONTEXT.md`) that helps agents decode project-specific terminology.

<details>
<summary>Example</summary>

Here's an example of what a `CONTEXT.md` entry looks like. Which one is easier to read?

- **BEFORE**: "There's a problem when a lesson inside a section of a course is made 'real' (i.e. given a spot in the file system)"
- **AFTER**: "There's a problem with the materialization cascade"

This concision pays off session after session.

</details>

This is built into [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) — a grilling session that builds a shared language with the AI and documents hard-to-explain decisions in ADRs.

> [!TIP]
> A shared language has many other benefits than reducing verbosity:
>
> - **Variables, functions and files are named consistently**, using the shared language
> - As a result, the **codebase is easier to navigate** for the agent
> - The agent also **spends fewer tokens on thinking**, because it has access to a more concise language

### #3: The Code Doesn't Work

> "Always take small, deliberate steps. The rate of feedback is your speed limit. Never take on a task that's too big."
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**The Problem**: Even when the agent understands what to build, it still produces broken code when it has no feedback on how that code actually runs.

**The Fix**: Tight feedback loops — static types, browser access, and automated tests.

For automated tests, a red-green-refactor loop is critical. The [`/tdd`](./skills/engineering/tdd/SKILL.md) skill enforces this, giving the agent clear guidance on what makes good and bad tests.

For debugging, the [`/diagnose`](./skills/engineering/diagnose/SKILL.md) skill wraps best debugging practices into a disciplined loop.

### #4: We Built A Ball Of Mud

> "Invest in the design of the system _every day_."
>
> Kent Beck, [Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> "The best modules are deep. They allow a lot of functionality to be accessed through a simple interface."
>
> John Ousterhout, [A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**The Problem**: Agents accelerate software entropy. Codebases get more complex at an unprecedented rate.

**The Fix** is caring about the design of the code:

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md) quizzes you about which modules you're touching before creating a PRD
- [`/zoom-out`](./skills/engineering/zoom-out/SKILL.md) tells the agent to explain code in the context of the whole system
- [`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) helps rescue a codebase that has become a ball of mud — run it once every few days

### Summary

Software engineering fundamentals matter more than ever. These skills are a best effort at condensing those fundamentals into repeatable practices. Enjoy.

## Reference

### Engineering

Skills for daily code work.

- **[diagnose](./skills/engineering/diagnose/SKILL.md)** — Disciplined diagnosis loop for hard bugs and performance regressions: reproduce → minimise → hypothesise → instrument → fix → regression-test.
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — Grilling session that challenges your plan against the existing domain model, sharpens terminology, and updates `CONTEXT.md` and ADRs inline.
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — Find deepening opportunities in a codebase, informed by the domain language in `CONTEXT.md` and the decisions in `docs/adr/`.
- **[setup-skills](./skills/engineering/setup-skills/SKILL.md)** — Scaffold the per-repo config (issue tracker, triage label vocabulary, domain doc layout) that the other engineering skills consume. Run once per repo before using `to-issues`, `to-prd`, `triage`, `diagnose`, `tdd`, `improve-codebase-architecture`, or `zoom-out`.
- **[tdd](./skills/engineering/tdd/SKILL.md)** — Test-driven development with a red-green-refactor loop. Builds features or fixes bugs one vertical slice at a time.
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** — Break any plan, spec, or PRD into independently-grabbable GitHub issues using vertical slices.
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** — Turn the current conversation context into a PRD and submit it as a GitHub issue. No interview — just synthesizes what you've already discussed.
- **[triage](./skills/engineering/triage/SKILL.md)** — Triage issues through a state machine of triage roles.
- **[zoom-out](./skills/engineering/zoom-out/SKILL.md)** — Tell the agent to zoom out and give broader context or a higher-level perspective on an unfamiliar section of code.

### Productivity

General workflow tools, not code-specific.

- **[caveman](./skills/productivity/caveman/SKILL.md)** — Ultra-compressed communication mode. Cuts token usage ~75% by dropping filler while keeping full technical accuracy.
- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — Get relentlessly interviewed about a plan or design until every branch of the decision tree is resolved.
- **[write-a-skill](./skills/productivity/write-a-skill/SKILL.md)** — Create new skills with proper structure, progressive disclosure, and bundled resources.
