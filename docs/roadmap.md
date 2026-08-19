# Tools, properties, open threads, and what is under watch

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

### 12.2 Cairn holds the work that changes the witness supply

Two units of work in the first adopting repository changed no claims and changed which claims are **possible**.
Integrating a type-aware linter created a lint witness supply, so every rule the project enables became a
development claim with a working witness ([§5.2](./claims.md#52-the-four-kinds)). Adopting an infrastructure
framework moved [§4.1](./claims.md#41-the-subject-must-be-rederivable-from-the-repository)'s boundary and raised
the ladder for every infrastructure claim at once.

Neither is an **amendment**: §7 defines one as a set of claim changes, and neither proposes any. Neither is
**fog**: both are statable, so §10 refuses them. Neither earns a **rationale** under
[§11.2](./lifecycle.md#112-when-to-write-one-and-when-to-ship-it)'s three tests. So both produced no artifact at
all, and the second needed a migration plan long enough to be worth writing down, which went to a scratch file.

**The stakes are inverted, which is why this needed an owner.** A claim change moves one promise. A supply change
moves the ceiling for every promise that comes after it, including ones nobody has thought of yet. §7 gates the
first, and nothing recorded the second.

**It belongs to cairn, and not to a fourth amendment operation.** Work that raises the supply is planned before
there is a branch, which is exactly where cairn already sits ([§12.1](#121-cairn-keeps-its-state-outside-the-repository-it-serves)), and its
consequence is not a claim change but a **re-examination**: the ladder moved, so every claim sitting below the new
ceiling is now worth revisiting, and the fog that was blocked on the old ceiling may be clear. That is a queue
operation on material cairn already holds. Modelling it inside the amendment would put a repository-wide event
into an artifact scoped to one unit of work.

Two things follow, and both are cairn's to design:

- **Cairn notices the plan, not the merge.** The trigger is a planned adoption, so the signal arrives while the
  work is still fog. A watcher that found it afterwards would be reporting a ceiling that already moved.
- **The output is a review list, never a rewrite.** Same constraint as the watcher above: cairn proposes what to
  re-examine and writes nothing into the repository.

Crux itself learns nothing from this. [§13.1](#131-two-tools-in-order) sequences the tools crux will build; the
tooling **around** crux is what decides what the catalog can hold, and that asymmetry is the finding.

## 13. What gets built first, and what is being watched

### 13.1 Two tools, in order

**1. Crux.** It reads and lists claims, witnesses, and rationale links. Its whole scope is already stated above:

- note which files carry `@glossary`, so that a changed one earns its row on the readout ([§3.3](./vocabulary.md#33-a-changed-glossary-is-a-yellow-row))
- list the claims, each with its kind and the witnesses that attest it
- list the markers, each with its claims, its extent, and its scope
- list, for each claim, the rationale that ground it ([§11.1](./lifecycle.md#111-what-a-rationale-grounds))
- report the form errors of [§6.6](./format.md#66-form-errors) — unattested, orphaned, dead scope, mixed, misplaced, unknown kind, collision,
  forward dangle
- report the two non-errors of [§6.6](./format.md#66-form-errors) — a rationale grounding a deleted claim, and a changed glossary
- emit the marker index as a machine form, for adapters and for cairn's watcher to join against

It runs nothing. It stores nothing.

**Crux also ships a short form of this document, and that is a requirement rather than a courtesy.** The first
build had to recover the marker grammar, the grounding rules, and the amendment
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

**§13.3's exit condition is met, and it has now been met three times.** The first project built under this
vocabulary that is not crux itself produced repeated pain points at the drawing board, a second set when it built
what it had designed, and a third when it built a second package against a live external service. All of them are
now in this documentation rather than in a log:

| Repeated                                           | Found     | Where it landed                                                                                                                                              |
| -------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| every cross-reference is grep and hope             | designing | §12.1 — cairn holds each fact once, derives every view                                                                                                       |
| the cost of clearing fog varies wildly             | designing | [§10](./lifecycle.md#10-fog) — a fog item records what would clear it                                                                                        |
| a claim was reworded after its witness was written | building  | [§7](./lifecycle.md#7-the-amendment) — an amendment is a specification, not a freeze                                                                         |
| the amendment changed during the build             | building  | [§9.1](./review.md#91-the-sequence) — the builder escalates, and the operator rules                                                                          |
| a green canvass held unsound witnesses             | building  | [§9.2](./review.md#92-the-builders-exit-condition) — green is a handoff, and [§8.6](./review.md#86-why-the-canvass-and-the-audit-stay-separate) was measured |
| witnesses were regrouped and coverage judged late  | building  | [§7.1](./lifecycle.md#71-judge-coverage-while-the-amendment-is-still-text) — judge coverage while the amendment is text                                      |
| work changed what claims are **possible**          | both      | §12.2 — cairn holds the work that changes the witness supply                                                                                                 |

**The building rounds did something the designing round could not: they corrected the framework itself.** [§5.9](./claims.md#59-group-claims-by-the-failure-a-reader-can-see)
retracts a rule that this document carried, and the retraction came from reading a real catalog and finding it
unreadable. No amount of design produced that. It needed fourteen claims, an audit, and an operator who had to
review the result.

**The third round moved the boundary rather than a rule.** Adopting an infrastructure framework put declared
infrastructure inside the checkout, which returned four claims that
[§4.1](./claims.md#41-the-subject-must-be-rederivable-from-the-repository) had deleted — and showed that the
document had recorded the answer of the day as a principle. That is a different kind of correction from §5.9's,
and it is the one that produced §12.2: the tooling **around** crux decides what the catalog can hold.

**The rounds also changed where the evidence comes from.** The first two rounds found defects in the **claims**.
The third found them in the **witnesses** — a witness observing a proxy, a witness green while doing nothing, a
mechanism nothing observed at all. That is the shift that produced the runbooks, because a defect in a witness is
caught by procedure at the moment of building rather than by a rule in a specification.

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
20. A witness that has never denied has not been tested.
21. A witness that observes a proxy for its subject says nothing about the subject.
22. An amendment is read as a whole. Its claims can be individually defensible and jointly false.
23. The ceiling of the witness ladder is set by the ecosystem and by the budget, not by the claim alone.

Property 1 is scoped on purpose. Property 16 creates a legitimate class of work that has no claim and never will —
the infrastructure a repository is deployed onto — and that work belongs in a runbook, with the risk named there.
Pretending otherwise is how a permanently green witness gets written.

Properties 17 to 19 came from building rather than from designing, and 18 and 19 are the pair that sets a claim's
altitude. 18 pushes a claim up and 19 pushes it down. Neither is safe alone. See [§5.8](./claims.md#58-coverage) and [§5.9](./claims.md#59-group-claims-by-the-failure-a-reader-can-see).

Properties 20 to 23 came from the third round, and they are about the **witness** where 17 to 19 are about the
claim. 20 and 21 are the two ways a witness can be green and worthless — it never fired, or it fired at the wrong
thing — and both are caught by procedure rather than by a rule, which is why the
[builder's runbook](./runbooks/builder.md) exists.

**Do not renumber this list.** [§3.2](./vocabulary.md#32-crux-does-not-read-it) cites property 11 and [§11.3](./lifecycle.md#113-settled-by-construction-and-the-index-that-does-not-exist) cites properties 2 and 15. New properties are
appended.

## 15. Open threads

Each thread says what it blocks, because [§10](./lifecycle.md#10-fog)'s exit gate does work that a bare question does not — writing down
_what does this block_ is what surfaces a contradiction, and it surfaced one here.

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
- **Whether a repository's prefixes stay coherent, and what notices when they do not.** _Blocks nothing, and it is
  the cost §3.4 accepted knowingly._ With no projects, a prefix is a naming convention that nothing checks. A typo
  is still caught — as **orphaned** or **unattested** — but a second scheme growing beside the first is not. The
  candidates are a house rule, a lint witness over the catalog, or a report that lists prefixes by frequency and
  lets a person see the drift. Nothing decides between them yet, and a repository large enough to have the problem
  does not exist.

**Closed since the last revision.**

_Does belay receive an amendment, or a branch with failing witnesses?_ — **an amendment.** The thread rested on
one argument: a witness that denied before the build is the only mechanical evidence that it can deny at all. The
third round produced that evidence without a gate. A builder breaks each witness and watches it fire
([builder's runbook](./runbooks/builder.md), step 4), which needs none of the independence an audit needs and
costs one edit per witness. It also covers what the gate could not: three of the four kinds have no failing state
before the build, and this reaches all of them. The gate would have bought less, later, and would have demanded a
complete amendment that no design phase can deliver.

_Where the work that changes the witness supply is recorded_ — §12.2, in cairn, because it is planned before there
is a branch and its consequence is a re-examination rather than a claim change.

_Whether a claim may have another claim's instrument as its subject_ — **no.** It is at the wrong altitude
([§5.9](./claims.md#59-group-claims-by-the-failure-a-reader-can-see)), and the real question underneath it —
_is the instrument still running?_ — belongs to the canvasser
([§8.2](./review.md#82-the-join-from-a-marker-to-a-verdict)).

_Where fog is stored_ — §12.1, outside the target repository, with a watcher.

_Whether a single-project repository carries a prefix_ — dissolved. §3.4 deleted projects, so no slug carries one.

_Whether this document's §2 is the root project's glossary_ — dissolved with projects. §2 is a glossary like any
other, and it carries `@glossary` because it settles the words its claims are written in.

_Where the catalog sits_ — anywhere. A claim is declared by `@claim` wherever that directive appears, so the
catalog is defined by the directive rather than by a location, and crux still resolves no path.

## 16. Under watch

An observation that has occurred **once** is an anecdote. §13.3 is explicit that one occurrence redesigns nothing
and a repeat is evidence, so this section exists to keep a single sighting without letting it become a rule.

**Nothing here is normative.** A rule in §1–§14 is what crux says. An entry here is what somebody saw. An entry
leaves this section in one of three directions: it repeats and becomes a rule, it is contradicted and is deleted
with a line saying so, or it stays here indefinitely because it was true once and never again.

A few of these are already load-bearing as **procedure**, in the runbooks. That is deliberate and it is not a
contradiction: a prompt that costs a reviewer one question is worth running on one sighting, where a rule that
every adopting repository must satisfy is not.

| #   | Observation                                                           | Seen                       | Now                           |
| --- | --------------------------------------------------------------------- | -------------------------- | ----------------------------- |
| W1  | a required credential that no observation depends on                  | one tool, one provider     | argued by hand                |
| W2  | a witness needs a working directory that nothing gives it             | one test harness           | a comment, and a fragile line |
| W3  | an exactly-once claim hid a commit point between two stateful systems | one external side effect   | a reviewer prompt             |
| W4  | a mechanism was added, argued for, and observed by nothing            | one amendment, three cases | a reviewer prompt             |
| W5  | an amendment has no vintage                                           | three unenacted amendments | recorded only                 |
| W6  | one prose sentence became two claims at two altitudes                 | one sentence               | recorded only                 |
| W7  | fog cleared and produced no artifact at all                           | one item                   | recorded only                 |

### W1 — A tool can be capable of an offline witness and still refuse to run one

One infrastructure framework resolves its cloud credentials when the client is constructed, which is before it
knows that every resource in the stack will be emulated. On a checkout with no credentials the failure is an
authentication error at construction, and the witness file never executes — although nothing it would observe
comes from the account. The repair was four lines of environment holding placeholders that the tool validates for
shape and never authenticates. Non-functional placeholders are also the safer setting, because a run that escapes
local mode then fails to authenticate rather than deploying into a real account.

**The judgment it needed has no vocabulary in §4.1**: is a witness that requires a credential it never
authenticates with still rederivable from a checkout? The answer taken was yes, because the credential is not an
input to the attestation — no observation the witness makes depends on its value. A tool that offers no way to
tell _required_ from _used_ leaves that argument to a person. **It becomes evidence if a second tool moves the
same boundary for the same reason.**

### W2 — A witness can need something from the process, not from the program

A resource declaration used a path relative to its own package on purpose, because an absolute path computed at
load time gets written into shared state and makes a second checkout plan a pointless update. A real deploy pins
the directory by running from that package. A test has none of that: it deploys two stacks together and runs from
a third directory, where the relative path is nothing. The only instrument available was to change the working
directory at module scope, before either deploy.

It works, it is one line, and it is the sort of line somebody deletes while tidying. **The shape is what is worth
keeping**: a witness can have a requirement that is neither a service nor a fixture, and that no harness has a
slot for. A dependency system models what a witness needs **inside** the program and nothing about the process
around it — working directory, environment variables, the scratch directory the run writes into. Recorded because
the next repository to hit it may be one where the repair is not one line.

### W3 — Exactly-once language hid an unobservable commit point

A claim promised that at most one send returns for a local date, and every witness affirmed it: the fires ran in
sequence, the binding was observed, and each stopped once the success was written. An independent audit asked what
is true **between** those two observations. The send and the write have no shared transaction and the send has no
idempotency key, so a send that returns followed by a write that fails leaves the repository in the same state as
a send that never returned. A later fire must retry and risk a duplicate, or stop and risk nothing being sent.

The claim was not under-covered by a missing test. **Its guarantee was not implementable with the declared
primitives**, and the repair was to lower it ([§5.8](./claims.md#58-coverage)) to the protocol that is observable.
Coverage produced the right answer once the question was asked, so no rule is proposed. The prompt is in the
[reviewer's runbook](./runbooks/reviewer.md), step 4. **A second occurrence will show whether this belongs beside
property 21 or stays an ordinary coverage failure.**

### W4 — A mechanism can be added, argued for, and never witnessed

A fix commit added a per-attempt identifier — a migration, a unique index, a generated value, and a paragraph of
the amendment arguing for it. No witness named it. Replacing the generated value with a constant string passed all
twenty-three tests. The consequence was not cosmetic: the upsert keyed on that identifier folded a day's three
refusals into one row holding the first failure's time and the last one's reason, which is a claim about recording
failures quietly recording one of them.

**Coverage did not catch it and is not supposed to.** Coverage asks whether a claim's witnesses reach the claim,
and they did. The identifier is in the **implementation of** the claim, introduced to keep the claim true under a
retry added mid-build. Crux's audit walks from claims to witnesses, and nothing walks the other way to ask _what
here is load-bearing and unobserved_. Two other gaps in the same amendment had the same shape, and in all three
the prose was ahead of the instruments — and the prose is what an audit reads. The prompt is in the
[reviewer's runbook](./runbooks/reviewer.md), step 5. **A second occurrence would argue for making it a step in
the model rather than a habit in a runbook.**

### W5 — An amendment has no vintage

Three amendments were authored under [§5.9](./claims.md#59-group-claims-by-the-failure-a-reader-can-see)'s
retracted predecessor and then sat unenacted while the framework moved. Nothing in the repository marked them
stale: no rule version, no form error, no marker. They were revised three sessions later because a person
remembered.

Crux checks claims against witnesses continuously and checks amendments against the framework never. An amendment
is the one artifact that sits unenacted long enough for the framework to move underneath it — a rationale
grounding a deleted claim is the same shape at the other end of the pipeline
([§11.5](./lifecycle.md#115-a-dangling-grounding-in-both-directions)), reported and not an error.

**Deliberately filed low.** An underspecified framework moving under an unenacted artifact is expected rather than
defective, and this stops mattering the moment crux stabilises. Recorded so the observation is not lost.

### W6 — One prose sentence, two claims, two altitudes

One sentence of a design document — _the token gives no access to the history_ — became two claims at different
altitudes, neither implying the other, with no honest single witness spanning both. A granularity signal, and one
occurrence says nothing yet about where the right altitude is.

### W7 — Fog can clear without producing anything

[§10](./lifecycle.md#10-fog) gives fog two exits: write the claim and assign its witness, or discharge into a
rationale ([§11.6](./lifecycle.md#116-fog-can-discharge-into-a-rationale)). One item found a third. It asked
whether a confirmation page shows a chart of recent history; the answer was no, and no artifact resulted, because
an existing claim already forbade the alternative for an unrelated reason.

So the item was fog by the definition — the claim could not be written — and clearing it added nothing to the
catalog, the rationale directory, or the tracker. It stopped being open.

**Put to the operator, who declined to model it**, and that is right twice over:
[§11.2](./lifecycle.md#112-when-to-write-one-and-when-to-ship-it)'s three tests reject a rationale here, so
declining generates nothing, correctly. Worth watching whether this is common. If it is, a tracker needs a
disposition for _answered, already covered_ that does not look like an abandoned item — and the fact that an
existing claim answered it is small evidence that the claim was written at the right altitude.
