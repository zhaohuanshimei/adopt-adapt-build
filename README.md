# adopt-adapt-build

`adopt-adapt-build` is an agent skill for making disciplined build-vs-buy decisions before writing custom code.

It helps an agent resist the default "just code it" instinct by forcing an explicit decision:

- `Adopt`: use an existing tool or library
- `Adapt`: wrap, extend, or selectively fork an existing tool
- `Build`: implement from scratch only after existing options are rejected

## Why This Exists

Agents are often good at writing code quickly, but that makes them prone to rebuilding things the ecosystem already solves.

This skill standardizes a safer workflow:

1. understand the real need
2. search for credible existing solutions
3. evaluate them with the same rubric
4. make an explicit `Adopt / Adapt / Build` recommendation
5. only then proceed to implementation

The result is less duplicated effort, better reuse of mature OSS, and more transparent dependency decisions.

## What The Skill Does

When invoked, the skill tells the agent to:

1. clarify the problem and constraints
2. search for official tools and mature OSS candidates
3. shortlist the strongest options
4. compare them on functional fit, integration fit, operational fit, maintenance, security, license, and cost
5. recommend `Adopt`, `Adapt`, or `Build`
6. describe the next implementation step only after the decision is explicit

## When To Invoke It

Use this skill when:

- a user asks to implement a feature that may already be solved by an existing library, framework, CLI, or platform-native tool
- a task involves common engineering domains such as auth, search, testing, observability, UI primitives, API tooling, parsing, deployment, background jobs, or infrastructure setup
- the agent is about to introduce a new dependency and needs a structured justification
- the agent is tempted to write a substantial custom implementation from scratch

## What Good Output Looks Like

The skill is meant to produce a compact decision artifact:

- the real problem statement
- a shortlist of candidates
- a consistent evaluation summary
- one recommendation: `Adopt`, `Adapt`, or `Build`
- a concrete next step

See [examples/decision-template.md](file:///Users/celongzhao/20260424_NewsDigest/standalone-skills/adopt-adapt-build/examples/decision-template.md) for the expected output shape.

## Install

Once this directory is published as its own GitHub repository, install it with:

```bash
npx skills add https://github.com/OWNER/adopt-adapt-build --skill adopt-adapt-build
```

Replace `OWNER` with your GitHub username or organization.

## Repository Layout

```text
adopt-adapt-build/
├── SKILL.md
├── README.md
├── CHANGELOG.md
├── LICENSE
├── .gitignore
└── examples/
    └── decision-template.md
```

## Maintenance Notes

- `SKILL.md` is the canonical behavior definition
- `README.md` explains positioning, installation, and repository usage
- `CHANGELOG.md` tracks skill evolution
- `examples/` contains reusable reference material

This repository is intentionally lightweight so it is easy to publish, version, and reuse across multiple agent environments.

## Publishing Checklist

1. Create a new GitHub repository named `adopt-adapt-build`
2. Copy this directory into that repository root
3. Review `LICENSE` and adjust the copyright holder if needed
4. Push the repository to GitHub
5. Install and test with `npx skills add ... --skill adopt-adapt-build`
6. Verify the installed skill triggers before implementation tasks where ecosystem reuse is likely

## License

This repository currently ships with the MIT License.
