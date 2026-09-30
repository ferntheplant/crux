# Checks

What the core tool, crux, calculates from one checkout. It reads the catalog and the witnesses, it reports form errors, and it names the audit scope. It runs nothing, it stores nothing, and it depends on no other tool ([Tool set](tools.md#tool-set)).

## Form errors

A machine finds each form error with no tool, no language, and no intelligence. **The person who causes a form error must be able to repair it when they cause it.**

> [!CLAIMS]
>
> ### Unattested
>
> Crux reports each claim that no witness attests.
>
> ### Orphaned
>
> Crux reports each `@attests` slug that names no claim.
>
> ### Collision
>
> Crux reports two headings with one slug in one file. A claim heading and a section heading with one slug are also a collision.
>
> ### Malformed block
>
> Crux reports a claims block whose first block after its first line is not a heading, a claim with no claim text, claim text that holds a block other than a paragraph, and a claims block before the first H2 of its file.
>
> ### Dangling link
>
> Crux reports each link, in each Markdown file that git tracks, whose path names no file that git tracks, or whose fragment names no heading in the Markdown file that it names.
>
> ### Unsupported version
>
> Crux reports each Markdown file whose `catalog` value is not a version that it can read, and it reads no claim from that file.
>
> ### Located
>
> Each form error names the file and the line of each thing that causes it.

**Crux compares text. It never reads the file system.** Each check compares one thing that crux read with a different thing that crux read. A path in a link resolves against the list of files that git tracks ([Links](catalog.md#links)).

**A collision is local to one file.** A slug holds the path of its file, so a heading slug must be unique only in its file ([Unique headings](catalog.md#unique-headings)). The first version of this file also reported two catalog files with one file name, because a slug held only the file name. A path cannot collide, so that error is deleted.

**The long form had _mixed_,** a block with both a noun and a verb directive. There is one directive now, so the error cannot occur.

## Index

The index is what crux knows about one checkout. Crux calculates it again on each run.

> [!CLAIMS]
>
> ### Claims view
>
> `crux claims` lists each claim in catalog order, under its headings, with its slug and the witnesses of each.
>
> ### Witnesses view
>
> `crux witnesses` lists each witness with its extent and its slugs.
>
> ### Machine form
>
> `crux index --json` writes the index to standard output. Each catalog file has its path and its version. Each claim has its slug, file path, line range, label, and a hash of its claim text. Each witness has its file, extent, and slugs.
>
> ### Summary
>
> `crux summary` prints a one-page account of the format, for an agent to read at the start of a session.
>
> ### Deterministic
>
> One checkout gives the same output, byte for byte, on each run.
>
> ### Read-only
>
> Crux writes no file in the repository, and it keeps no cache that changes its output.

The machine form is for adapters and for cairn ([Cairn](tools.md#cairn)). A readout is never committed ([Readout](readout.md#readout)), and neither is an index.

## Scope

Given the machine index of the merge base and of HEAD, crux names the audit scope. It compares text by slug, never by position.

> **Audit scope** = witnesses whose instrument changed **∪** witnesses that attest a claim whose text changed.

> [!CLAIMS]
>
> ### Claim change
>
> When the claim text of a claim changes, each witness of that claim is in scope.
>
> ### Instrument change
>
> When the extent of a witness changes, that witness is in scope.
>
> ### Prose reopens nothing
>
> A change that is only in prose puts no witness in scope. Crux lists the headings whose prose changed, for the triage of the judge.
>
> ### Rename
>
> When a slug disappears and a new slug appears with the same claim text, crux reports a rename. A renamed claim is not a claim change.
>
> ### Rename in a witness
>
> When the only change in the extent of a witness is an `@attests` slug, from an old slug to its new slug in a reported rename, that witness is not in scope.

The first term of the scope is the most important. A builder that adds a test to a claim outside the amendment sets its own measure of success, and the instrument change puts that test in front of the auditor.

**Crux detects a rename whether or not a crux command did it.** The detection needs identical claim text, after the whitespace is normalised. Thus it can miss a rename, and it cannot invent one. A missed rename is a deletion and an addition, and each witness of the claim is in scope. That is over-attribution, which is permitted ([Over-attribution](readout.md#over-attribution)). A witness that nobody updated after a rename is orphaned, and the form check finds it.

**Three edits rename a claim, and none of them opens an audit.** A label edit renames it, because the label is a name and its slug is the slug of the heading ([Claim heading](catalog.md#claim-heading)). A move to a different file renames it. A move of its file to a different directory renames it, because the path is part of the slug ([File path](catalog.md#file-path)). The cost that stays is the edit to each `@attests` and each link that cites the old slug.

**A section heading edit renames nothing.** No section heading is part of a slug. It only makes the heading links to it dangling, and the form check finds them.

**A rename and a text change in one diff are a deletion and an addition.** The scope compares the merge base with HEAD, so it sees only the sum of a pull request. Put a large rename in its own pull request.

> [!NOTE] **Open: the diff base.** This blocks belay. The merge base is the clear answer, and it needs git.

### Scope as a directive

The long form had `@scope <globs>`, which named the subject of a witness. It is deleted.

**A judgment replaces its one mechanical use.** Crux used `@scope` to keep an inferential verdict when the diff touched no glob. Now the judge does a triage of the diff ([Verdict](readout.md#verdict)). Thus the directive had no function. Its other users were the reading list of the auditor and the claims that a diff affects. An intelligence always read those, and the prose of the witness supplies them.

**A manual dependency list was the wrong instrument.** The code already tells what it depends on. A list beside the code becomes incorrect in the unsafe direction: a helper that nobody listed changes, and the verdict stays green.

**The cost of the deletion.** A kept verdict was arithmetic. _Affirms (unaffected)_ is a judgment. A wrong judgment is quiet, but people examine a wrong verdict. Thus the readout shows each result of the triage ([Triage shown](readout.md#triage-shown)).
