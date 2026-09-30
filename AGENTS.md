# crux

<!-- What the project is belongs in ABSTRACT.md, not here. Everything from the House rules section down ships with the baseline template and is meant to be kept. -->

## Where things live

| If you need                 | Read                                    |
| --------------------------- | --------------------------------------- |
| What this project is        | [`ABSTRACT.md`](./ABSTRACT.md)          |
| What a word means           | [`CONTEXT.md`](./CONTEXT.md)            |
| How the framework works     | [`spec/`](./spec/)                      |
| What to do in a session     | [`runbooks/`](./runbooks/)              |
| Why a rule reads as it does | the prose beside its claims — see below |

New writing goes to one of those homes from the start, and **nothing lives in two of them**.

**The reasoning stays beside the rule it justifies.** This repository has no separate rationale directory: a rejected alternative, a retracted rule, and a deleted design are written into the section that replaced them, because a rule split from its argument is an assertion nobody can weigh. The runbooks are the one exception — they are read per session and carry no history.

**The specification is written in its own format.** The files in `spec/` are catalog files: each claim is a heading inside a `> [!CLAIMS]` block, and everything outside those blocks is prose that never promises. Read [`spec/catalog.md`](./spec/catalog.md) before you add or change a claim.

**Links are the citation form.** In Markdown, cite a claim or a heading with an ordinary relative link, `[Rename](checks.md#rename)`, so it resolves on GitHub and in VS Code. In commits and trackers, write the claim's slug — its path from the repo root plus the heading slug, `spec/checks.md#rename` — which is the same token an `@attests` line uses.

**Write the specification and the runbooks in ASD-STE100 Simplified Technical English.** Short sentences, active voice, and one meaning for each word. [`CONTEXT.md`](./CONTEXT.md) defines the technical words.

## House rules

- **Conventional Commits, always.** The allowed types are in [`commitlint.config.ts`](./commitlint.config.ts); CI checks every commit on a PR and the PR title, because a squash merge takes the title as the subject.
- **`vp run ready:agent` is the gate.** It runs the checks listed in the `ready` script in [`package.json`](./package.json) and prints `ready: ok`, or only the output of the steps that failed. A step whose output runs long is trimmed, and when the full output could be saved, the `lines omitted` marker names the file holding it; read it when the trimmed part matters. The README's _What runs where_ table says what the git hooks and CI add around the gate. A change is not done until it passes from a clean checkout.
- **Gate commands live in `package.json`, not in `run.tasks`.** `vp run` reads both, and `run.cache: true` already caches scripts, so a task wrapper adds only `dependsOn` and per-task `cache.env`/`cache.input` control — nothing a linear `check → test → build` chain needs. Scripts stay visible to pnpm, CI, and editors, and a task name can live in only one place. Define a `vite.config.ts` task when it needs cross-package ordering, env-sensitive caching, or no caching at all (`ready:agent`, whose comment says why).
- **Absolute imports across modules.** `../**` is a lint error; sibling imports are fine.
- **No `any`, no non-null assertions, no floating promises.** These are lint errors, not preferences. If a rule seems wrong for this repo, change it in [`vite.config.ts`](./vite.config.ts) with a comment saying why — do not suppress it inline.
- **Dependencies come from the catalog.** Shared versions live in [`pnpm-workspace.yaml`](./pnpm-workspace.yaml); packages depend on `catalog:`. sherif fails the gate on any warning; its config is the `sherif` key in the root `package.json`, which ignores only `non-existant-packages`, because the template declares `packages/*` before anything lives there.
- **Secrets never reach git.** gitleaks runs where the README's _What runs where_ table says. A false positive goes in `.gitleaksignore` by fingerprint, so the exception is reviewed like any other change.
- **Spelling is checked.** A word typos flags that is correct here (a tool's name, a domain term) goes in [`_typos.toml`](./_typos.toml) with a comment saying what it is.
- **Dead code gets deleted.** `vp exec fallow` reports what nothing reaches, and an unused file, export, or dependency fails the gate. A file that is only reachable at runtime belongs in `fallow.toml`; everything else it flags is real.

## Definition of done

A change is done when its production path is reachable through a real entrypoint; success and expected failure are tested; `vp run ready:agent` passes from a clean checkout; and the documentation is updated where implementation invalidated an assumption — a new term means a [`CONTEXT.md`](./CONTEXT.md) entry, and a change in what the project is or is not means an `ABSTRACT.md` edit.

## Skills

[`.agents/skills/`](./.agents/skills/) holds this repo's skills. `.claude/` is a symlink to `.agents/`, and `CLAUDE.md` is a symlink to this file, so every agent reads one set of instructions. `CLAUDE.md` is gitignored and created on install by [`scripts/link-agents.mjs`](./scripts/link-agents.mjs) — the file's own header says why.

<!--VITE PLUS START-->

# Using Vite+, the Unified Toolchain for the Web

This project is using Vite+, a unified toolchain built on top of Vite, Rolldown, Vitest, tsdown, Oxlint, Oxfmt, and Vite Task. Vite+ wraps runtime management, package management, and frontend tooling in a single global CLI called `vp`. Vite+ is distinct from Vite, and it invokes Vite through `vp dev` and `vp build`. Run `vp help` to print a list of commands and `vp <command> --help` for information about a specific command.

Docs are local at `node_modules/vite-plus/docs` or online at https://viteplus.dev/guide/.

## Built-in Commands vs Scripts

`vp <name>` runs a built-in command. `vp run <name>` runs a `package.json` script or a `vite.config.ts` task. Scripts cannot overwrite built-ins, so `vp dev` and `vp run dev` may do different things. Check `package.json` and `vite.config.ts` first, and run `vp run <name>` when the project defines a script or task with that name.

## Tool Versions

Run `vp toolchain` to show versions and relationships in the active Vite+
release. Add a tool name to select part of the graph. For example, run
`vp toolchain vite`. Use `--global` to ignore the local `vite-plus` package. Use
`vp why <package>` to show the package-manager dependency graph.

## Review Checklist

- [ ] Run `vp install` after pulling remote changes and before getting started.
- [ ] Run `vp check` and `vp test` to format, lint, type check and test changes.
- [ ] Check if there are `vite.config.ts` tasks or `package.json` scripts necessary for validation, run via `vp run <script>`.
- [ ] If setup, runtime, or package-manager behavior looks wrong, run `vp env doctor` and include its output when asking for help.

<!--VITE PLUS END-->
