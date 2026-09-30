# crux

Crux is a format that records what a codebase promises. A machine can find each promise, and a person can then decide if the work satisfies it. No code implements it yet; this repository holds the specification.

| Read                           | For                                        |
| ------------------------------ | ------------------------------------------ |
| [`ABSTRACT.md`](./ABSTRACT.md) | What crux is, what it refuses to do        |
| [`spec/`](./spec/)             | The full model, written as a crux catalog  |
| [`runbooks/`](./runbooks/)     | What a builder or a reviewer does per turn |
| [`AGENTS.md`](./AGENTS.md)     | House rules for humans and agents          |

## Setup

The toolchain comes from the [baseline](https://github.com/ferntheplant/baseline) template: Vite+ for the JavaScript tooling, mise for gitleaks, typos, and Node. Its README covers the one-time machine setup and the GitHub repository settings.

```bash
mise trust && mise install
vp install
```

## Daily commands

```bash
vp run ready       # the gate: typos, vp check, sherif, tests, builds, fallow
vp run ready:agent # the same gate for agents: `ready: ok`, or only the failing steps' output
vp check --fix     # format + autofix lint
```
