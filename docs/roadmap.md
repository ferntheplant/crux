# Tools, properties, and what is open

Part of the crux framework documentation. [Contents](./README.md) · [Abstract](../ABSTRACT.md)

---

## 12. The tools

The format is the product. The tools sit on it and are replaceable.

|      | Name       | Does                                                                |
| ---- | ---------- | ------------------------------------------------------------------- |
| core | **crux**   | defines and parses claims, markers, rationale links, and amendments |
| 1    | **cairn**  | holds fog; turns fog into an amendment                              |
| 2    | **belay**  | takes an amendment; produces a PR that is cheap to rule on          |
| 3    | **beacon** | attention router; moves the operator between fog work and rulings   |

**The format and the core share the name crux.** The format is the product, and the core is the only thing that
reads it, so there is nothing left for a second name to distinguish. Say _a crux catalog_, _a crux marker_.

One line is hard, and the rest are soft: **crux takes no dependency on cairn, on belay, or on beacon.** Delete any
of the three, and crux still works. The reason is the cost of error. The format goes into other repositories that
you do not control, so a change to it is a migration you cannot run. The three tools are your own code, and you
can divide them again at any time.

**Crux reads lines and resolves globs.** It knows no language, no runner, and no linter, and it never resolves a
path. Everything that needs that knowledge is an adapter, and adapters live in belay. A glossary and a rationale
cost it nothing beyond what it already does: it finds one by name and the other by directive, and it reads the
contents of neither.

**A tracker stores slugs. A tracker never stores claim text.** Whoever has the checkout resolves the slug. This is
what makes a tracker outside the repository possible. The interface is small: **a tracker emits an amendment.**

### 12.1 Cairn keeps its state outside the repository it serves

That interface is not a concession to an external tracker. It is the interface designed for one, and two things
settle that it must be used.

**An authorable amendment has nowhere to live before there is a branch.** An amendment is held by the branch and
enacted by the merge ([§7](./lifecycle.md#7-the-amendment)), but amendments exist — and are worth ordering against each other — before any work
starts. _A file on the branch_ cannot be the whole answer when there is no branch yet.

**Fog inside the target repository pollutes product history.** Every clarification of a fog item becomes a commit
interleaved with commits that change the product. This was observed rather than predicted, and it is the argument
for a second store keyed to the target.

**The bill comes due immediately, and it is worth stating before the tool is built.** When fog lives in the
repository, a slug rename is atomic with the code that caused it. Moving the tracker out breaks that atomicity, so
a dangling reference now lives in a different system, on a different release cycle, that cannot see the rename
happen.

**The first build showed the same fact from the builder's side, and added one case.** A claim slug lived in the
amendment, in the rationale that grounds it, in the catalog, and in the markers, and one split changed all four by
hand. It also showed that **the number of claims in an amendment is not stable**: the tracker said ten, the
amendment held eleven, and the build finished with fourteen ([§7](./lifecycle.md#7-the-amendment)). A count is a view, so cairn derives it and never
stores it. A stored count is wrong from the first escalation.

**So cairn needs a watcher, and it is a repair rather than a feature.** It checks out the target at its main
branch, runs crux, and warns about stale references in its own tickets and fog. Three constraints:

- **It holds no facts.** Re-derive from HEAD every run, like [§9.3](./review.md#93-the-supervisor)'s supervisor. Kill it, restart it, lose nothing.
- **It consumes what crux already emits.** §13.1's machine-form marker index. The interface is a file, not an API,
  and crux learns nothing about cairn.
- **The warnings point one way only.** Cairn warns about cairn's references, never about the repository. The
  repository is authoritative. A watcher reporting _into_ the repository would quietly create the dependency the
  hard line above forbids.

**The symmetry is worth naming.** [§4.1](./claims.md#41-the-subject-must-be-rederivable-from-the-repository) ruled that reality drifting from the repository is monitoring rather than
review, and pushed it out of crux. The watcher is monitoring, for tracker drift from the repository. Same shape,
different subject, excluded from crux for the identical reason, and landing in cairn because cairn is the thing
that can be wrong.

## 13. What gets built first, and what is being watched

### 13.1 Two tools, in order

**1. Crux.** It reads and lists claims, witnesses, and rationale links. Its whole scope is already stated above:

- find every `GLOSSARY.md` and read its `@project`, which is the set of projects and the only input the checks
  below cannot derive from the markers themselves ([§3.4](./vocabulary.md#34-a-glossary-declares-a-project))
- list the claims, each with its kind and the witnesses that attest it
- list the markers, each with its claims, its extent, and its scope
- list, for each claim, the rationale that ground it ([§11.1](./lifecycle.md#111-what-a-rationale-grounds))
- report the form errors of [§6.6](./format.md#66-form-errors) — unattested, orphaned, dead scope, mixed, misplaced, unknown kind, misfiled,
  collision, unfounded, forward dangle
- report the two non-errors of [§6.6](./format.md#66-form-errors) — a rationale grounding a deleted claim, and a changed glossary
- emit the marker index as a machine form, for adapters and for cairn's watcher to join against

It runs nothing. It stores nothing.

**Crux also ships a short form of this document, and that is a requirement rather than a courtesy.** The first
build had to recover the marker grammar, the project declaration rule, the grounding rules, and the amendment
lifecycle by reading this repository at the start of the session. This document is long on purpose — it holds the
arguments — and nobody reads it per session. So the short form states the rules and none of the reasoning, and it
links here for the reasoning. It belongs with crux and not with cairn: it is about the format, and cairn holds the
queue.

**2. A belay MVP.** A local supervisor that goes from an amendment on a branch to a PR with a posted readout:

- run the sources named in `.belay/witnesses.toml`, map each report through its adapter, and join on the marker
  index ([§8.2](./review.md#82-the-join-from-a-marker-to-a-verdict))
- produce the readout ([§8.4](./review.md#84-the-readout))
- open or update the PR, and post the readout as a comment naming its commit ([§9.4](./review.md#94-where-the-readout-is-published))
- loop the builder and the reviewer, with a fresh reviewer each cycle and a cycle budget ([§9.3](./review.md#93-the-supervisor))

It does **not** hold fog, and it is not a tracker or a router.

### 13.2 The tracker was deliberately not third, and the period has now closed

Cairn was left off this list on purpose: several threads in §15 were about fog, none had an answer, and a tracker
built then would have encoded guesses about material nobody had handled under this vocabulary. So the two tools
above are built **by hand at the fog layer** — every amendment authored the slow way, every conversion from fog to
a claim done by a person with an agent. That was the experiment.

**§13.3's exit condition is met, and it has now been met twice.** The first project built under this vocabulary
that is not crux itself produced repeated pain points at the drawing board, and then produced a second set when it
built what it had designed. All of them are now in this document rather than in a log:

| Repeated                                           | Found     | Where it landed                                                                                                                                              |
| -------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| every cross-reference is grep and hope             | designing | §12.1 — cairn holds each fact once, derives every view                                                                                                       |
| the cost of clearing fog varies wildly             | designing | [§10](./lifecycle.md#10-fog) — a fog item records what would clear it                                                                                        |
| a claim was reworded after its witness was written | building  | [§7](./lifecycle.md#7-the-amendment) — an amendment is a specification, not a freeze                                                                         |
| the amendment changed during the build             | building  | [§9.1](./review.md#91-the-sequence) — the builder escalates, and the operator rules                                                                          |
| a green canvass held unsound witnesses             | building  | [§9.2](./review.md#92-the-builders-exit-condition) — green is a handoff, and [§8.6](./review.md#86-why-the-canvass-and-the-audit-stay-separate) was measured |

**The building round did something the designing round could not: it corrected the framework itself.** [§5.9](./claims.md#59-group-claims-by-the-failure-a-reader-can-see)
retracts a rule that this document carried, and the retraction came from reading a real catalog and finding it
unreadable. No amount of design produced that. It needed fourteen claims, an audit, and an operator who had to
review the result.

**So cairn is designable, and it is still not second.** The order stands, because belay is what makes the catalog
worth having and cairn is what makes it comfortable. What changed is that cairn's design no longer waits on
evidence — it waits on a queue.

### 13.3 The dogfooding rule

> **While building tools 1 and 2, the pain of tracking fog and of converting fog into an amendment is the data.
> Record it as it happens.**

This applies to the human and to every agent in the work. An agent that notices one of the following states it in
its report, and does not silently work around it:

| Watch for                                                | What it tells you                                                                               |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| a claim that could not be written, and what unblocked it | where the fog boundary of [§10](./lifecycle.md#10-fog) actually falls                           |
| a claim that was written and then reworded               | the right granularity for a claim                                                               |
| a witness assignment that changed the design             | [§7](./lifecycle.md#7-the-amendment) — the altitude was wrong, and the witness is what found it |
| an amendment that had to change during the build         | how much design must precede a build, and where the entry gate belongs                          |
| what had to be re-derived at the start of a session      | exactly what the tracker must hold                                                              |
| a catalog that was tiring to read                        | the altitude is wrong, whatever each single claim looks like                                    |
| a by-hand step that felt **clerical**                    | a candidate for the tool to absorb                                                              |
| a by-hand step that felt like **thinking**               | never automate it                                                                               |

The last two are the test that matters. Clerical work is a missing feature. Thinking is the work.

**Watch the reader, and not only the writer.** The sixth row was added after the first build, and it is the row
that found [§5.9](./claims.md#59-group-claims-by-the-failure-a-reader-can-see). Every claim in that catalog was defensible on its own. The catalog as a whole was not, and the
only signal was an operator saying that reviewing it felt tedious. A complaint about the reading is evidence about
the design, and it is easy to dismiss because it names no single defect.

**Two entries in the never-automate column are settled.** [§11.3](./lifecycle.md#113-settled-by-construction-and-the-index-that-does-not-exist) holds the first: _claim, or settled by
construction?_ has no mechanical form and never will. [§5.8](./claims.md#58-coverage) holds the second: whether a claim's witnesses reach it
is a judgment, and no count of sound standings substitutes for it.

**Where the notes go.** One append-only file, `FOG-LOG.md`, in the project doing the building. It is fog by
definition, so nothing in it is a claim, and none of it is durable. What survives review is lifted into this
document, and the log keeps the trail.

**When the period ends.** Not when the tools are finished. It ends when the log holds a pain point that has
**repeated**. One occurrence is an anecdote and it redesigns nothing; a repeat is evidence. §13.2 records that this
has now happened once, and the rule does not retire — it applies again to every project that adopts the vocabulary,
because the second adopter tests what the first one could not.

## 14. The properties

1. Nothing **in the repository** is built without a claim, and nothing merges without being judged against one.
2. A claim is finished when something that is not you can falsify it.
3. Fog is named as fog and cleared deliberately, and what would clear it is recorded before the work starts.
4. Prefer artifacts that are legible in both directions — guidance going in, feedback coming out.
5. Evidence is indexed by claim, and generated rather than collected by hand.
6. Human attention is spent only on authority: set a claim, rule on an outcome, abort. Never on status.
7. Cheap re-entry over deep autonomy.
8. Durable state is plain files. No single tool owns them. Each tool derives its view and stores nothing.
9. The machine checks form. A human or a model checks truth.
10. The gate is applied by somebody who did not build.
11. Over-attribution is permitted. Under-attribution is not.
12. A directive exists only for what the core must resolve without intelligence.
13. Words are settled before the claims that use them.
14. The catalog holds the decision. A rationale holds the reasoning.
15. A claim earns its place only if something could plausibly violate it.
16. Crux judges the repository. What is not rederivable from a checkout is not a claim.
17. A witness that closes one way to fail does not affirm the way to succeed.
18. A claim names a failure a reader can see. A claim that describes its own witness is at the wrong altitude.
19. Every witness sound does not mean the claim is covered.

Property 1 is scoped on purpose. Property 16 creates a legitimate class of work that has no claim and never will —
the infrastructure a repository is deployed onto — and that work belongs in a runbook, with the risk named there.
Pretending otherwise is how a permanently green witness gets written.

Properties 17 to 19 came from building rather than from designing, and 18 and 19 are the pair that sets a claim's
altitude. 18 pushes a claim up and 19 pushes it down. Neither is safe alone. See [§5.8](./claims.md#58-coverage) and [§5.9](./claims.md#59-group-claims-by-the-failure-a-reader-can-see).

**Do not renumber this list.** [§3.2](./vocabulary.md#32-crux-does-not-read-it) cites property 11 and [§11.3](./lifecycle.md#113-settled-by-construction-and-the-index-that-does-not-exist) cites properties 2 and 15. New properties are
appended.

## 15. Open threads

Each thread says what it blocks, because [§10](./lifecycle.md#10-fog)'s exit gate does work that a bare question does not — writing down
_what does this block_ is what surfaces a contradiction, and it surfaced one here.

- **Does belay receive an amendment, or a branch with failing witnesses?** _Blocks belay's entry gate, so it blocks
  tool 2._ The strongest argument for the branch is not test-first discipline and is not the deleted classifier:
  **a witness that denied before the build is the only mechanical evidence that it can deny at all.** Every other
  soundness check is a person reading. Against it: three of the four kinds have no failing state, so the gate would
  read _every witness that can deny, does deny_, and the work moves rather than disappears. **The first build adds
  a third consideration and does not settle the thread.** The amendment changed three times during that build
  ([§7](./lifecycle.md#7-the-amendment)), so a gate that demands a complete and correct amendment is asking for something no design phase can
  deliver. That argues against a heavy gate of either shape, and [§9.1](./review.md#91-the-sequence)'s step 3 is the route that makes a light one
  survivable.
- **How the audit scope gets its diff base.** _Blocks tool 2, and nothing before it._ The merge base of the branch
  is the obvious answer, and it needs git. Whose job that is has no answer yet.
- **Where the amendment lives.** _Blocks nothing until cairn emits one._ A file on the branch is the current
  assumption, rendered into the PR description. §12.1 has since settled that cairn holds the amendment before there
  is a branch, so this thread is now only about the branch-side representation.
- **How a lint-rule witness denies before the build.** _Blocks the thread above, and only if it resolves toward the
  branch._ A new rule denies on untouched code, not in one place. Two candidates: an allowlist, or land the rule
  with the refactor.
- **The spelling of _canvass_.** _Blocks belay's command names, so it blocks nothing until tool 2 has a CLI._ One
  letter separates it from _canvas_, and the wrong one is also a real word. **poll** is the fallback, and it loses
  only the completeness property.
- **Beacon.** _Blocks nothing._ Unowned. Its payload is fully determined by the amendment and the readout, so it
  invents no data model and can be deferred.
- **Whether this document's [§2](./vocabulary.md#2-the-vocabulary) is the root project's glossary.** _Blocks the root project's first claim, because
  that claim needs a `GLOSSARY.md` with `@project root` in it ([§3.4](./vocabulary.md#34-a-glossary-declares-a-project))._ For every other repository the two are
  separate — the framework's words and the project's words. Here they collide, because the project is the
  framework. Keeping both would give one word two homes.
- **Where the catalog sits inside a project.** _Blocks nothing; it is a house rule either way._ [§3.4](./vocabulary.md#34-a-glossary-declares-a-project) draws
  `docs/catalog/`, and [§6.6](./format.md#66-form-errors) says crux never resolves a path, so nothing enforces it. Whether that freedom is worth
  having, or whether it only invites drift between repositories, has no answer yet.

**Closed since the last revision.** _Where fog is stored_ — §12.1, outside the target repository, with a watcher.
_Whether a single-project repository carries a prefix_ — [§3.5](./vocabulary.md#35-the-slug-carries-the-project), yes, always, and `@project` is what made it cheap.
