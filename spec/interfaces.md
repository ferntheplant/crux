# Interfaces

How the tools meet. Crux is the format and the core tool. Each other tool does one job in one layer, and it meets the other tools only through the messages in this file. Thus you can replace one tool, or do its work by hand, and change no other tool.

## Layers

| Layer | Tool | Does | Durable state |
| --- | --- | --- | --- |
| requirements | **crux** | reads the catalog; gives the index and the scope ([Checks](checks.md)) | the catalog, in the repository |
| requirements | **cairn** | holds fog and amendments; gives amendments ([Cairn](cairn/README.md)) | its records, outside the repository |
| measurement | **carabiner** | joins tool reports and answers to witnesses; makes the readout ([Carabiner](carabiner/README.md)) | none |
| orchestration | **belay** | takes an amendment; makes a pull request that is cheap to rule on ([Belay](belay/README.md)) | its records |
| execution | **bedrock** | gives workspaces and agent runs ([Bedrock](bedrock/README.md)) | workspaces and the run archive |
| attention | **beacon** | moves the operator between fog work and rulings | none |

**The operator is not a layer.** The operator gives authority that no tool has: the operator approves an amendment, decides an escalation, and makes the ruling. Each layer asks for this authority at a known point, and at no other time.

**Git is the shared record.** Each tool reads the main branch through the index. The merge is the only event that changes what the catalog promises, and each tool sees it in git, with no message from a different tool. Thus cairn learns that an amendment is enacted from the index of the main branch, not from belay ([Enactment](cairn/README.md#enactment)).

**Each layer can be a person or a file.** The test of the interfaces is that each tool can be absent:

| Absent | What does its work |
| --- | --- |
| cairn | The operator writes an amendment file and gives it to belay. |
| belay | A person builds. CI runs carabiner, and a different person writes the answers. |
| bedrock | Belay runs its agents in local worktrees. |
| carabiner | A person reads the tool reports and writes the readout by hand. |
| beacon | The operator reads the readouts and the cairn records directly. |

> [!CLAIMS]
>
> ### Index only
>
> A tool other than crux gets claims, witnesses, headings, and links only from the machine index of crux. It never parses a catalog file to find them.
>
> ### Backbone words
>
> Each message that one tool gives to a different tool uses only the words of the root `CONTEXT.md` and the words of the tool that owns the form of the message.
>
> ### File form
>
> Each message between tools has a form that a person can write and read in a file, with no tool.

**Why the index is the only door.** The format must stay easy to change ([Versions](catalog.md#versions)). If belay or cairn parsed Markdown, each change to the format would be a change to them. The index has its own version ([Index version](checks.md#index-version)), so the format can change behind it. An intelligence still reads catalog files as text. This rule is about what a tool calculates.

**Why the words are limited.** A tool can use its own words inside itself. Belay can use the words of a hospital, and bedrock can use the words of its provider. A message to a different tool is read by a tool that does not know those words. Thus the root glossary is the language between tools, and a tool glossary is a dialect inside one tool ([Settling the words](practice.md#settling-the-words)).

## Messages

| Message | From → to | The form is owned by | Specification |
| --- | --- | --- | --- |
| index | crux → each tool | crux | [Index](checks.md#index) |
| scope | crux → carabiner | crux | [Scope](checks.md#scope) |
| amendment | cairn → belay, carabiner | crux owns the entries; cairn owns the rest | [Amendment](#amendment) |
| escalation | belay → operator → cairn | crux | [Escalation](#escalation) |
| report | a tool → carabiner | that tool | [Adapters](carabiner/README.md#adapters) |
| answers | an intelligence → carabiner | crux | [Answers](readout.md#answers) |
| readout | carabiner → operator, belay, beacon | crux | [Machine form](readout.md#machine-form) |
| workspace, run | belay ↔ bedrock | bedrock | [Bedrock](bedrock/README.md) |

## Amendment

An amendment goes through three tools. Cairn holds it, belay builds it, and carabiner measures the work against it. Each tool calculates with a small part of it. Crux owns that part, and cairn owns the rest.

> [!CLAIMS]
>
> ### Amendment form
>
> An amendment is a JSON document with the keys `entries`, `prose`, and `version`. The holder of an amendment can add keys, and each other tool ignores them.
>
> ### Entry
>
> An entry is one operation on one slug: `add` with the proposed claim text, `change` with the new claim text, or `delete` with no text.
>
> ### Proposed slug
>
> The slug of an `add` entry names a claim that does not exist yet. A tool that checks slugs never reports it as a slug that names no claim.
>
> ### Opaque prose
>
> The prose of an amendment is Markdown that no tool calculates with. An intelligence reads it.
>
> ### Version
>
> The version of an amendment is the SHA-256 of a JSON document that holds only its `entries` and its `prose`, with sorted keys and no whitespace between tokens.
>
> ### Renames are not entries
>
> A rename that crux reports is outside each amendment. The comparison of an amendment with the catalog changes ignores it.

```json
{
  "version": "sha256:4c1f…",
  "entries": [
    {
      "op": "add",
      "slug": "api/sessions.md#expired-token",
      "text": "When a request carries an expired session token, the API returns 401 and changes no stored data."
    },
    { "op": "delete", "slug": "api/legacy.md#cookie-auth" }
  ],
  "prose": "Why the work is necessary, the witness that will attest each entry, and the coverage of each claim…"
}
```

**Why crux owns the entries.** An entry predicts one change to the catalog. Crux already calculates the changes to the catalog between two commits ([Catalog changes](checks.md#catalog-changes)). Thus the question _does the pull request do what the amendment says?_ is a comparison, not a judgment ([Amendment match](readout.md#amendment-match)). Only the prose and the code stay a judgment.

**What cairn owns.** Where an amendment is kept, its name in a tracker, who approves it and how, the form in which a person writes it, its order among other amendments, its fog, and the structure of its prose. Cairn can change each of these, and no other tool changes. A cairn can keep an amendment as a Linear issue and give the JSON form only at the interface.

**Why the version is a hash.** Belay must know when somebody changed an amendment during the work. A hash lets each tool calculate the version, with no question to the holder and with no holder at all. An earlier design kept a copy of the text of the issue and compared strings. The hash compares the same things, and it needs no copy.

**Why a rename is not an entry.** A rename changes no promise ([Rename](checks.md#rename)). An amendment states what the catalog promises. Crux finds each rename, so an entry for it adds nothing that a tool needs.

**The writing practice is not the format.** An amendment names the witness that will attest each claim ([Amendments](practice.md#amendments)). That plan is in the prose, because only an intelligence reads it and the plan changes during the build.

## Escalation

An escalation is how the work asks the operator to change the amendment.

> [!CLAIMS]
>
> ### Escalation form
>
> An escalation is a JSON document with the keys `version`, `proposed`, and `reason`. The `version` is the version of the amendment that it questions. The `proposed` is a complete amendment. The `reason` is prose.
>
> ### Escalation decision
>
> When the operator accepts an escalation, its proposed amendment becomes the amendment, with a new version. When the operator refuses it, the amendment and its version do not change.

**The decision goes to the holder.** The holder of the amendment records the decision. The tool that escalated reads the current version again, and it learns the decision from it. Thus no tool calls back a different tool.

**A complete amendment, not a change.** The `proposed` key holds the full amendment that the escalation wants. Each tool can calculate its version, and the operator reads one document.

## Execution boundary

> [!CLAIMS]
>
> ### No claim words below orchestration
>
> No message between belay and bedrock has a slug, a claim, a witness, or a verdict as a field. Bedrock gives the instructions of an agent run to the agent as text that it does not read.

**Why.** Bedrock gives a place to work and an agent to do the work. If bedrock knew claims, you could not use a different bedrock without a change to the format, and you could not use bedrock for work that has no catalog. The words of bedrock are in its own glossary ([Bedrock words](bedrock/CONTEXT.md)).

## Where a claim lives

The root of `spec/` holds the format, the core tool, and each claim that each implementation of a layer must keep. A directory in `spec/` holds the claims of one tool, and it can hold the glossary of that tool. Thus you can change the specification of belay, and you change no file at the root and no file of a different tool.

**A tool directory is a group, not a project.** A slug already holds the path of its file, so a claim in `spec/belay/` has a different identity from a claim at the root, and nothing new is necessary ([Groups](catalog.md#groups)). The three failed designs of a project tried to give a claim a second identity. A directory gives it none. When a tool moves to its own repository, its directory moves, and each claim in it is a rename.

> [!NOTE] **Open: links between repositories.** When a tool moves to its own repository, its links to the root claims become URLs. Crux does not check a link with a scheme ([Link](catalog.md#link)). Nothing then reports a link that a rename made incorrect.
