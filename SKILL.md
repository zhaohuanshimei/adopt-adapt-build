---
name: "adopt-adapt-build"
description: "Evaluates existing OSS, libraries, and tools before coding. Invoke when a feature, integration, or infrastructure task may be solved by adopting, adapting, or avoiding a dependency."
---

# Adopt, Adapt, Build

Use this skill to avoid unnecessary custom implementation work.

The default stance is:

1. Search for mature existing solutions first
2. Evaluate whether one should be adopted directly
3. If not, evaluate whether one can be adapted safely
4. Only then recommend building from scratch

## When To Use

Invoke this skill when:

- A user asks to build a feature that might already exist as an OSS library, framework, CLI, service, or component set
- A task involves auth, search, parsing, background jobs, API tooling, UI primitives, testing infrastructure, deployment tooling, observability, or other common engineering problems
- You are about to write a substantial amount of new code and there is a realistic chance that the ecosystem already solves most of the problem
- You are considering introducing a new dependency and need a disciplined way to justify it

Do not skip this skill just because you already know how to hand-write the solution.

## Hard Gate

Before implementing a new capability, you MUST do one of the following explicitly:

- Recommend `Adopt`
- Recommend `Adapt`
- Recommend `Build`

You must not jump directly from "user wants X" to "here is custom code for X" without first documenting why an existing solution is not appropriate.

## Core Workflow

### 1. Clarify The Need

Capture the minimum facts required to evaluate options:

- What problem must be solved?
- Is this product-facing, infrastructure-facing, or internal tooling?
- What stack must it fit into?
- What constraints matter most: speed, control, bundle size, security, license, long-term maintenance, offline use, self-hosting?
- What is the acceptable dependency weight?

If the requirement is still vague, ask clarifying questions before searching.

### 2. Search Existing Solutions

Search in this order when relevant:

1. Official tools from the underlying platform or framework
2. Widely adopted OSS libraries and frameworks
3. Existing tools already present in the repo or organization
4. Smaller niche tools only if the first three tiers do not fit

Prefer evidence, not intuition. Use real signals such as:

- Active maintenance
- Clear documentation
- Release cadence
- Installation footprint
- Community adoption
- Compatibility with the project's stack

### 3. Build A Shortlist

Create a shortlist of 2-5 candidates when possible.

For each candidate, capture:

- What it solves
- What it does not solve
- Integration shape
- Runtime and build implications
- License
- Maintenance signals
- Security or supply-chain concerns

If no serious candidates exist, say so explicitly instead of forcing a comparison.

### 4. Evaluate With The Same Rubric

Assess candidates consistently across these dimensions:

- **Functional fit**: does it solve the real problem, not just part of it?
- **Integration fit**: does it match the current language, framework, deployment model, and architecture?
- **Operational fit**: what does it add in runtime complexity, hosting needs, CI/CD burden, or observability needs?
- **Maintenance fit**: is it likely to remain healthy and understandable over time?
- **Security fit**: does it introduce risky scripts, questionable provenance, or a large attack surface?
- **License fit**: is the license acceptable for the project?
- **Cost fit**: is integration cheaper than custom implementation over the expected lifetime?

### 5. Make One Of Three Decisions

#### Adopt

Choose `Adopt` when:

- A candidate already solves most of the problem
- It fits the stack and constraints well
- The added dependency burden is justified

Output:

- Chosen tool
- Why it wins
- Any guardrails for using it safely
- Minimal integration plan

#### Adapt

Choose `Adapt` when:

- A candidate solves a large portion of the problem
- Some wrapping, extension, patching, or selective forking is still needed
- Adapting is clearly cheaper and safer than starting from zero

Output:

- Base tool
- What will remain upstream
- What will be wrapped, patched, or overridden locally
- Where the boundary between vendor code and project code will live
- Upgrade and maintenance implications

#### Build

Choose `Build` when:

- No candidate has acceptable functional and operational fit
- Existing tools create more lock-in, complexity, or risk than value
- The problem is sufficiently project-specific that a custom solution is the simpler long-term choice

Output:

- Why existing solutions were rejected
- What the custom implementation must own
- What future reevaluation trigger would justify revisiting ecosystem options

## Output Format

When using this skill, present findings in this shape:

### Problem

- One short statement of the real need

### Candidates

- Candidate A
- Candidate B
- Candidate C

### Evaluation

- Fit
- Risks
- Trade-offs

### Recommendation

- `Adopt`, `Adapt`, or `Build`

### Next Step

- If `Adopt` or `Adapt`: describe the integration plan before implementation
- If `Build`: describe the custom design scope before implementation

## Preferred Behavior

- Prefer the smallest sufficient solution, not the most feature-rich one
- Prefer official or highly credible ecosystem tools over obscure packages
- Prefer wrapping a stable tool over copying large amounts of vendor code into the repo
- Prefer explaining rejection reasons clearly when you choose `Build`
- Prefer local seams that keep replacement possible later

## Red Flags

Slow down if you notice any of these:

- "I already know how to write this from scratch"
- "It will be faster if I just code it"
- "This package is popular, so it must fit"
- "We can evaluate license and security later"
- "Let's install first and think later"

These are exactly the moments where this skill should be applied.

## Examples

### Example: Authentication

User asks for login, sessions, password reset, and OAuth.

- Search for framework-native auth options and mature auth libraries first
- Compare self-hosted and library-based options
- Recommend `Adopt` or `Adapt` unless the project has unusually custom auth rules

### Example: Search

User asks for full-text search with ranking and filters.

- Compare database-native search, hosted search, and OSS engines
- Recommend `Adopt` if one matches scale and ops constraints
- Recommend `Build` only if search needs are narrow enough that custom SQL is simpler

### Example: UI Component Set

User asks for tabs, dialogs, forms, and command menus.

- Search for mature component primitives and design-system options first
- Recommend `Adopt` or `Adapt` before inventing a component system by hand

## Final Rule

The goal of this skill is not "always add dependencies."

The goal is to make build-vs-buy decisions explicit, evidence-based, and repeatable.
