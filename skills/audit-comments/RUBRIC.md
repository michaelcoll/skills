# Comment rubric

How to judge one comment block. A documented repo rule overrides anything here.

## What counts as a block

A **block** is a run of consecutive comment lines attached to the same thing (the item below it, or the statement beside it). One block, one verdict. A doc comment and the plain comment under it are two blocks.

Out of scope, neither judged nor counted:

- Tool directives: `eslint-disable`, `@ts-expect-error`, `#[allow(…)]`, `swiftlint:disable`, `prettier-ignore`, `noqa`, `type: ignore`… Their trailing justification is in scope when it has one.
- Structure markers: `MARK:`, `#region` / `#endregion`.
- License and copyright headers, SPDX lines.
- Shebangs, pragmas, and magic comments: `#!/usr/bin/env`, `// swift-tools-version`, `"use client"`, `# frozen_string_literal`…

## The six axes

Ask each question of every block, against the code it describes. Read that code, and only that code: the enclosing item in full (the function, type, or module the block documents or sits in), plus whatever the comment names (a symbol, a file, a value, a caller):

1. **Exactness**: is it still true of the code it describes?
2. **Information**: does it say something the code, the names, and the types do not already say?
3. **Why over what**: does it explain an intent, an external constraint, a trap, or an invariant?
4. **Durability**: will it stay true with no reference to a PR, an issue, an author, or a past state of the code?
5. **Audience**: is it API documentation read by callers elsewhere (`///`, `/** */`, JSDoc, DocC, docstrings), or a local implementation note?
6. **Reading cost**: is its length in proportion to what it brings?

## Reasons and verdicts

Each block gets one reason from this closed list. The reason fixes the verdict.

| Reason           | Axis          | Verdict   | When                                                                                                                                               |
| ---------------- | ------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `stale`          | Exactness     | `Rewrite` | False or partly false about the current code. `Delete` when nothing true is left worth saying.                                                     |
| `todo`           | -             | `Ticket`  | A `TODO`, `FIXME`, `HACK`, or `XXX`. `Delete` when the work is done or no longer makes sense.                                                      |
| `commented-code` | -             | `Delete`  | Dead code kept in a comment: git has it. `Ticket` when the surrounding note says it is meant to come back.                                         |
| `history`        | Durability    | `Delete`  | Tells the story of a change ("added for #123", "we used to…", "now uses X instead of Y"). `Rewrite` when it holds a durable _why_: keep only that. |
| `paraphrase`     | Information   | `Delete`  | Restates the code, the name, or the signature. `Rewrite` when a repo rule or a lint requires a doc there: make it say what the signature does not. |
| `too-long`       | Reading cost  | `Rewrite` | Useful, but longer than what it brings. The note carries the shorter text.                                                                         |
| `api-doc`        | Audience      | `Keep`    | API documentation that adds to the signature: units, errors, side effects, invariants, preconditions, an example.                                  |
| `why`            | Why over what | `Keep`    | A local note that explains an intent, an external constraint, a trap, or an invariant the code cannot show.                                        |

When several reasons fit, take the first one in the table: it sets the most urgent action. A stale paraphrase is `stale`; a history note that is too long is `history`.

## Hard cases

- A doc comment on a public item that only restates the signature ("Returns the user id") is `paraphrase`, not `api-doc`: audience alone does not earn a `Keep`.
- A link to an external spec, an RFC, a vendor bug, or a workaround's upstream issue is `why`: it explains a constraint from outside the code. A link to the repo's own issue tracker that only says where the code came from is `history`.
- A comment that names the unit, the range, or the format of a value the type does not encode (`// seconds`, `// 0..=100`, `// ISO 8601`) is `why` when it is short, even though it looks like a paraphrase.
- A section comment that labels a group of statements (`// Validate input`) is `paraphrase` when the code below reads as plainly; a sign the code wants a function with that name.
