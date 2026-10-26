# How I work with AI coding tools

I use Claude Code every day as a working partner on a large TypeScript monorepo. I don't use it to autocomplete lines. I use it to run a whole delivery loop: understand, design, build, verify, review, ship. I stay in charge of scope and quality, and the tool does the heavy reading, writing and checking. This document describes that loop, the tools around it and the habits that make it reliable.

## 1. The loop

1. **Understand first.** Before any code, I have the assistant read the existing code, the rules files and the related tickets, and tell me what already exists and what is missing. I often ask for a survey with a table of "exists / missing / size" before I decide what to build.
2. **Brainstorm and design.** For anything new I use the *brainstorming* skill to turn the idea into a short design with its trade-offs. For bigger changes the design becomes a written record in the repo. Nothing is implemented until I approve the approach.
3. **Plan.** I use a planning step (the *writing-plans* skill, or a build spec in repos that use the BMad workflow) so the work is broken into small tasks with acceptance criteria and a list of the files that will change.
4. **Build in an isolated worktree.** Every ticket gets its own git worktree and branch, so parallel work never collides and the main checkout stays clean.
5. **Verify before claiming done.** I expect evidence: tests that ran, a build that passed, and for UI work a real screenshot from a running local environment. "It should work" is not accepted. The *verification-before-completion* habit is built into how I ask.
6. **Review.** Code review runs as a separate pass (several independent reviewers, then triage), and for risky areas a human signs off before merge.
7. **Ship through a pull request.** Small, logical commits with a required message format, a clear description of what changed, what was checked and what was deliberately not done.

## 2. Skills and tools I rely on

- **Superpowers skills:** brainstorming, writing-plans, executing-plans, subagent-driven development, dispatching parallel agents, using git worktrees, test-driven development, systematic debugging, requesting and receiving code review, verification before completion, finishing a development branch.
- **Project workflow skills (BMad):** build from an approved spec, code review, walkthrough, sprint planning and similar. Each has checkpoints where a human approves.
- **Repo rule files:** the project's own instructions (architecture decisions, naming, commit format, what is forbidden) are loaded into every session, so the assistant follows the team's rules without being reminded.
- **Persistent memory:** I keep short notes the tool reads in every session: how I like to work ("decide, don't quiz me", "list cut candidates and let me choose"), environment quirks of my machine, the state of long-running tickets, and rules like "never commit real customer data".
- **Hooks and CI as guardrails:** commit hooks run lint, type checks, tests and architecture guards on every commit. The rule is simple: never bypass a hook. If it fails, fix the cause.
- **Issue tracker and docs connectors:** the assistant reads and updates tickets and documents directly, so the status stays truthful without me copying things around.

## 3. Parallel agents, with control

For large work I split it into independent jobs and run several sub-agents in the background: for example one writes documentation drafts, one builds a feature in its own worktree, one fixes merge conflicts on a pull request. I give each job:

- an explicit scope and the files it may touch,
- the rules it must follow,
- what "done" means and what to report back.

I watch their progress, resume a job that hit a limit or a network error, and stop one immediately if it is going the wrong way. Jobs that touch the same files are never run at the same time. Anything that needs a human decision (authorization changes, merging, publishing to shared documents) is held for me.

## 4. Quality and honesty habits

- **Proof over promises.** For UI features I ask for numbered screenshots from a real run, and for AI features I ask for results from a real model, not a stub.
- **"How do you know?"** When the assistant states a fact (something is merged, a test is missing), I ask for the evidence. It goes back to the code or the pull-request API. Wrong claims get corrected in the document, not hidden.
- **Honest gaps.** I want every deliverable to say what is not done. I'd rather read a short "missing" list than a confident summary.
- **Small, reviewable output.** I cut lists and documents to the items that matter, and I decide what is a must-have for production versus a feature that can wait for a customer request.
- **Security and data discipline.** No real customer data in the repo, fictional fixtures only, authorization changes named explicitly in the commit and pull request, and secrets never in files.

## 5. What I do myself and what I delegate

I decide the goal, the scope, the priorities and what "good enough" means. I make the product and risk calls, review the important documents line by line, and press merge on risky changes. I delegate the reading, the drafting, the repetitive edits, running long test suites, producing documentation and screenshots, and chasing down merge conflicts.

## 6. Typical day

Check the status of open pull requests and tickets, start a new ticket in a fresh worktree, brainstorm and approve a design, let the assistant build and verify it, run documentation and review jobs in parallel, fix whatever the hooks and CI find, and finish with a short, honest status: what is merged, what is waiting, what needs my decision.

## 7. Example (generic)

Porting a legacy feature into a new architecture: survey the old and new code, write down what already exists, agree the plan, build the missing pieces in a separate worktree, run the real flow in a local environment with a real model, take screenshots as proof, open a pull request with the authorization changes called out, and keep the pull request mergeable as the main branch moves. The same loop applies to adding an AI agent, writing a production-readiness checklist, or fixing a flaky pipeline.
