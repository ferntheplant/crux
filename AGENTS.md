## Where things live

| If you need                 | Read                                    |
| --------------------------- | --------------------------------------- |
| What this project is        | [`ABSTRACT.md`](./ABSTRACT.md)          |
| How the framework works     | [`spec/`](./spec/)                      |
| What to do in a session     | [`runbooks/`](./runbooks/)              |
| Why a rule reads as it does | the prose beside its claims — see below |
| The full history of a rule  | `git show 27db6c6:docs/README.md`       |

New writing goes to one of those homes from the start, and **nothing lives in two of them**.

**The reasoning stays beside the rule it justifies.** This repository has no separate rationale directory: a rejected alternative, a retracted rule, and a deleted design are written into the section that replaced them, because a rule split from its argument is an assertion nobody can weigh. The runbooks are the one exception — they are read per session and carry no history.

**The specification is written in its own format.** The files in `spec/` are catalog files: each claim is a heading inside a `> [!CLAIMS]` block, and everything outside those blocks is prose that never promises. Read [`spec/catalog.md`](./spec/catalog.md) before you add or change a claim.

**Links are the citation form.** In Markdown, cite a claim or a heading with an ordinary relative link, `[Rename](checks.md#rename)`, so it resolves on GitHub and in VS Code. In commits and trackers, write the claim's slug — its path from the repo root plus the heading slug, `spec/checks.md#rename` — which is the same token an `@attests` line uses.

**Write the specification and the runbooks in ASD-STE100 Simplified Technical English.** Short sentences, active voice, and one meaning for each word. [`spec/glossary.md`](./spec/glossary.md) defines the technical words.

## House rules

- **Conventional Commits, always.** The allowed types are in [`commitlint.config.ts`](./commitlint.config.ts); CI checks every commit on a PR and the PR title, because a squash merge takes the title as the subject.
- **`vp run ready` is the gate.** It runs `vp check` (format, lint, type-check), then every package's `test`, then every package's `build`. A change is not done until it passes from a clean checkout.
- **A workspace package's `exports` resolves to source, not to `dist`.** The gate runs `check` and `test` before `build`, so on a clean checkout a consumer that resolves to `dist` fails on the gate's own ordering, before any code is wrong. The failure is confusing — a module resolution error in one package, caused by the order of three words in a script elsewhere. Everything here is TypeScript and nothing is published, so `build` stays as a check that the package packs rather than as the thing consumers use.
- **Gate commands live in `package.json`, not in `run.tasks`.** `vp run` reads both, and `run.cache: true` already caches scripts, so a task wrapper adds only `dependsOn`/`env`/ `input` control — nothing a linear `check → test → build` chain needs. Scripts stay visible to pnpm, CI, and editors, and a task name can live in only one place. Define a `vite.config.ts` task when it needs cross-package ordering or env-sensitive caching.
- **Absolute imports across modules.** `../**` is a lint error; sibling imports are fine.
- **No `any`, no non-null assertions, no floating promises.** These are lint errors, not preferences. If a rule seems wrong for this repo, change it in [`vite.config.ts`](./vite.config.ts) with a comment saying why — do not suppress it inline.
- **Dependencies come from the catalog.** Shared versions live in [`pnpm-workspace.yaml`](./pnpm-workspace.yaml); packages depend on `catalog:`.
- **Dead code gets deleted.** `vp exec fallow` reports what nothing reaches. A file that is only reachable at runtime belongs in `fallow.toml`; everything else it flags is real.

## Using Vite+, the Unified Toolchain for the Web

This project is using Vite+, a unified toolchain built on top of Vite, Rolldown, Vitest, tsdown, Oxlint, Oxfmt, and Vite Task. Vite+ wraps runtime management, package management, and frontend tooling in a single global CLI called `vp`. Vite+ is distinct from Vite, and it invokes Vite through `vp dev` and `vp build`. Run `vp help` to print a list of commands and `vp <command> --help` for information about a specific command.

Docs are local at `node_modules/vite-plus/docs` or online at https://viteplus.dev/guide/.

### Review Checklist

- [ ] Run `vp install` after pulling remote changes and before getting started.
- [ ] Run `vp check` and `vp test` to format, lint, type check and test changes.
- [ ] Check if there are `vite.config.ts` tasks or `package.json` scripts necessary for validation, run via `vp run <script>`.
- [ ] If setup, runtime, or package-manager behavior looks wrong, run `vp env doctor` and include its output when asking for help.
