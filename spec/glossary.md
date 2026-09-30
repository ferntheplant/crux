# Glossary

The words of crux. Each technical word has one meaning. The specification and the runbooks use each word only in that meaning, and they are written in ASD-STE100 Simplified Technical English.

## Words

| Word             | Meaning                                                                                                                                         |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **claim**        | One heading in a claims block and the paragraphs under it. A short statement that the codebase satisfies now. It is falsifiable.                |
| **label**        | The heading text of a claim. It is the name of the claim, not a part of its text.                                                               |
| **claim text**   | The paragraphs of a claim after its heading.                                                                                                    |
| **claims block** | A blockquote whose first line starts with `[!CLAIMS]`. It holds only claims.                                                                    |
| **catalog file** | A Markdown file that holds a claims block.                                                                                                      |
| **catalog**      | The set of all claims in the repository.                                                                                                        |
| **prose**        | Each part of a catalog file that is not in a claims block.                                                                                      |
| **heading slug** | The fragment that GitHub makes from the text of a heading. For example, `rename-in-a-witness`.                                                  |
| **slug**         | The identity of a claim: the path of its file from the root of the repository, `#`, and its heading slug. For example, `spec/checks.md#rename`. |
| **link**         | A CommonMark link with a relative path. It cites a file, a heading, or a claim.                                                                 |
| **rename**       | A slug that disappears in a diff, while a new slug appears with the same claim text.                                                            |
| **witness**      | A part of the repository that can show if the codebase satisfies a claim.                                                                       |
| **directive**    | One `@attests` and its token, on one line.                                                                                                      |
| **block**        | A contiguous sequence of directive lines.                                                                                                       |
| **extent**       | The lines that a block owns: from the block to its terminator, to the next block, or to the end of file.                                        |
| **attest**       | The relation that a witness records. Witness X attests claim Y.                                                                                 |
| **subject**      | The code that a witness examines. The witness observes it and does not take its contract from it.                                               |
| **instrument**   | The witness itself: its block, its extent, and the text of the claim that it attests.                                                           |
| **existence**    | If a claim has a witness. A form check finds it.                                                                                                |
| **verdict**      | What a witness tells about the subject: **affirms**, **affirms (unaffected)**, **denies**, or **silent**.                                       |
| **standing**     | If one instrument supports one claim: **sound**, **unsound**, or **unaudited**.                                                                 |
| **coverage**     | If the witnesses of a claim uphold it together: **covered**, **under-covered**, or **unaudited**.                                               |
| **amendment**    | The set of claim changes that one unit of work proposes.                                                                                        |
| **fog**          | Material that you want, but cannot yet write as a claim.                                                                                        |
| **canvass**      | To ask each witness for a verdict.                                                                                                              |
| **adapter**      | A converter from a verdict source into verdicts. The source is the report of a tool, or a judge.                                                |
| **judge**        | The intelligence that answers the inferential witnesses. It is the default adapter.                                                             |
| **triage**       | The one pass in which the judge reads a diff and selects what to ask again.                                                                     |
| **audit**        | To read instruments and set their standings, and then set the coverage of their claims.                                                         |
| **auditor**      | The intelligence that does the audit.                                                                                                           |
| **readout**      | The result of a canvass and an audit: one block for each claim.                                                                                 |
| **ruling**       | The decision that a human makes at the merge.                                                                                                   |

## Naming rule

Use a formal word for a **thing**. Use a plain word for an **event**. A person learns a thing once, so it gets a precise word. The objects in a sentence already describe an event. Thus the operations of an amendment are **add**, **change**, and **delete**, and a slug that changes is a **rename**.

## Rejected words

Record these words so that nobody proposes them again.

| Word                   | Rejected because                                                                                                                                                    |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **clause**             | It named a part of a claim. A claim is already the smallest promise, so one thing got two words.                                                                    |
| **marker**             | It named the same thing as _witness_. One thing gets one word.                                                                                                      |
| **project**            | It was tried in three forms, and all three failed. See [Groups](catalog.md#groups).                                                                                 |
| **run**                | You cannot run a prose witness.                                                                                                                                     |
| **poll**               | A poll samples. A canvass is complete. _Poll_ is the fallback if _canvass_ is too easy to misspell.                                                                 |
| **passes** / **fails** | _The witness fails_ can mean that the code is bad or that the witness is bad.                                                                                       |
| **ADR**                | An ADR holds the decision and the reasoning in one record. The catalog holds claims, with the reasoning as prose.                                                   |
| **complete**           | This was a name for _covered_. No audit can prove that nothing is missing.                                                                                          |
| **section number**     | `§8.2` was a second identity for a heading, and it did not move with the heading. A link cites the slug.                                                            |
| **pin**                | A ` ^id` at the end of a claim fixed its id. The heading of a claim now gives its slug, and GitHub and VS Code do not go to a pin. See [Claims](catalog.md#claims). |
| **id**                 | It named the part of a slug after `#^`. A claim now has a heading slug, which is a word that each viewer already uses.                                              |
| **wikilink**           | `[[file#heading]]` named a file by a short name, which can match several files. See [Links](catalog.md#links).                                                      |
| `@claim`               | Markdown structure declares a claim. The directive was a second name for a thing that the label already names.                                                      |
| `@claims`              | A directive with no token breaks the directive form. The `[!CLAIMS]` line does its work. See [Claims](catalog.md#claims).                                           |
| `@crux`                | A tool name in the format is a migration that you cannot run. See [Tool names](#tool-names).                                                                        |
| `@witness`             | `@attests` already opens a witness, so this directive added no information.                                                                                         |
| `@kind`                | Nothing used its closed set. The readout is in the order of the claims.                                                                                             |
| `@tag`                 | Free tags change without a signal. Declared tags need a registry. See [Groups](catalog.md#groups).                                                                  |
| `@glossary`            | Its only function was a readout row, and the row was noise. See [Settling the words](#settling-the-words).                                                          |
| `@grounds`             | It linked a rationale document to claims, and nothing mechanical read it. See [Prose](catalog.md#prose).                                                            |
| `@scope`               | It was a manual list of the dependencies of a witness. A judgment replaces its one use. See [Scope](checks.md#scope).                                               |
| a bare `@end`          | It is a keyword in Objective-C and Texinfo. A stray one cuts a witness short. Each terminator names its opener.                                                     |

## Settling the words

A claim is falsifiable only if its words are clear. Thus, settle the words before you write the claims that use them. Write the definitions. Select one word for each concept, and record the rejected synonyms. This is a convention, and crux does not check it.

**The glossary directive was deleted.** The long form had `@glossary`, and a change to a glossary gave a readout row. The row was usually noise. Add one word to a glossary, and each claim that can use that word gets a yellow row, but no claim changed its meaning. An operator learns to ignore a row like this, and an ignored row is worse than no row.

**The cost of the deletion.** A builder can make a definition narrower, and a claim becomes easier to satisfy. No claim and no witness changes, and nothing mechanical sees it. The reviewer reads the full diff, and the changed definition is in that diff. If a definition change alters what a claim promises, the claim text must also change, and changed claim text is already in the audit scope ([Claim change](checks.md#claim-change)).

## Tool names

The tools are **crux**, **belay**, **cairn**, and **beacon** ([Tool set](tools.md#tool-set)). A tool name has no meaning in the vocabulary. Thus you can rename a tool, and no migration is necessary.

For the same reason, no part of the format uses a tool name. The directive is `@attests`, the frontmatter key is `catalog`, and the block marker is `[!CLAIMS]`. The format goes into repositories that you do not control, and a tool name in it would make a rename into a migration that you cannot run.

> [!NOTE]
>
> Open: the spelling of _canvass_; it blocks the command names of belay. **Poll** is the fallback.
