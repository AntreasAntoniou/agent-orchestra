# Agent Orchestra

Agent Orchestra is an installable Agent Skill for designing multi-agent graphs: isolated
proposers, arbiters, adversaries, committees, recursive reviewers, cross-modal checks, and
saturation loops. Its central rule is simple: topology should follow the task's uncertainty and
stakes, not a fashionable agent count.

## Why use it?

Parallel agents only add evidence when they are genuinely independent. Agent Orchestra makes
isolation, adversarial refutation, explicit arbitration, provenance, and escalation first-class
parts of a graph. It also covers the filesystem discipline needed when agents produce code.

## Repository map

| Path | Purpose |
|---|---|
| [`SKILL.md`](SKILL.md) | Installable skill and full graph grammar |
| [`PLAYBOOK.md`](PLAYBOOK.md) | Compact operating reference |
| [`examples/saturating-review-engine.workflow.js`](examples/saturating-review-engine.workflow.js) | Worked JavaScript adapter for a runtime exposing `agent`, `parallel`, `pipeline`, `phase`, and `log` |
| [`variants/token-efficient/SKILL.md`](variants/token-efficient/SKILL.md) | Provider-neutral model-tier assignment overlay |

## Install

```sh
npx skills add AntreasAntoniou/agent-orchestra
```

The repository root is the skill directory. You can also copy or symlink it into the skills directory used by your agent host. For example:

```sh
ln -s "$(pwd)" ~/.agents/skills/agent-orchestra
```

Agent Skills-compatible hosts discover the skill through `SKILL.md`. Host-specific installation
paths and invocation syntax vary; consult your host's documentation.

## Runtime adapters

The orchestration patterns are runtime-neutral. A compatible adapter needs these capabilities:

- spawn an isolated agent with a prompt and label;
- run independent calls concurrently;
- pass structured outputs along dependency edges;
- validate machine-consumed results against a schema;
- provide isolated worktrees or disposable clones for code-producing nodes.

The included `.workflow.js` file is **one concrete adapter**, not the definition of the skill. It
expects a JavaScript workflow host that injects `agent`, `parallel`, `pipeline`, `phase`, `log`, and `args`. Port
the graph to another host by preserving its isolation and dependency edges, not by copying the
surface API literally.

## Minimal graph

```text
task -> two blind, read-only proposers -> arbiter -> one implementation
                                              -> fresh reality verifier
```

Use a single agent for deterministic low-risk work. Add agents only when they create independent
evidence, cover orthogonal failure modes, or protect an irreversible decision.

## Validation

```sh
python3 tests/validate_repo.py
node --check examples/saturating-review-engine.workflow.js
```

The JavaScript example is syntax-checked but requires a compatible workflow host to execute.

## Limitations

- Multiple calls to the same model are correlated; they are not automatically independent.
- An arbiter can confidently merge two plans that share the same false assumption.
- Agent output is untrusted input. Schema validation is necessary, not sufficient.
- This project documents orchestration patterns; it does not supply an agent runtime or sandbox.
- Claims such as orthogonality and emergence are operational heuristics, not peer-reviewed metrics.

## Contributing and security

See [`CONTRIBUTING.md`](CONTRIBUTING.md) and [`SECURITY.md`](SECURITY.md). Licensed under the
[MIT License](LICENSE).
