# crux

Crux is a format that records what a codebase promises. A machine can find each promise, and a person can then decide if the work satisfies it. No code implements it yet; this repository holds the specification.

| Read                           | For                                        |
| ------------------------------ | ------------------------------------------ |
| [`ABSTRACT.md`](./ABSTRACT.md) | What crux is, what it refuses to do        |
| [`spec/`](./spec/)             | The full model, written as a crux catalog  |
| [`runbooks/`](./runbooks/)     | What a builder or a reviewer does per turn |
| [`AGENTS.md`](./AGENTS.md)     | House rules for humans and agents          |

## Setup

The toolchain comes from the [baseline](https://github.com/ferntheplant/baseline) template: Vite+ for Node, pnpm, and the JavaScript tooling, and mise for gitleaks, typos, and agent-browser. [`BASELINE.md`](./BASELINE.md) covers the one-time machine setup, the lint presets, dependency versions, and the GitHub repository settings. [`.baseline.jsonc`](./.baseline.jsonc) records the baseline commit this repo last took; `/rebaseline` takes baseline's later changes.

```bash
vp env on          # once per machine, if you ever ran `vp env off`
mise trust && mise install
vp install
```

## Daily commands

```bash
vp run ready       # the gate: typos, vp check, sherif, tests, builds, fallow
vp run ready:agent # the same gate for agents: `ready: ok`, or only the failing steps' output
vp check --fix     # format + autofix lint
```
