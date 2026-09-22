# FAIR Lab planning

Issues in this repo are the FAIR Lab lines of effort. The board is the org project
[FAIR Lab Lines of Effort 2026](https://github.com/orgs/USAFA-AI-Center/projects/3).
The `loe/` directory holds the documentation that proves each line of effort was done.

## How an issue gets to Done

Nobody closes an issue by hand. A pull request closes it, and the pull request adds the
documentation. Merging the PR closes the issue and the board moves it to Done on its own.
That is the whole mechanism: no PR, no Done, and every Done has a write-up behind it.

There are two kinds of documentation, and the issue's `cadence:` label tells you which one
applies.

### One-off work (`cadence:once`): a completion record

One markdown file, written once, at the path in the issue's **Doc** line:

```
loe/NN-short-name.md
```

Copy `loe/TEMPLATE_completion_record.md`, fill it in, open a PR whose description says
`Closes #NN`. Merge closes the issue.

### Recurring work and standing responsibilities: a living document

The parent issue never closes. Its documentation is a folder:

```
loe/NN-short-name/
  README.md        how the program runs - kept current, edited by ordinary PRs
  2026-10.md       one dated record per recurrence
  2026-11.md
```

Each recurrence is a sub-issue of the parent (use the "Recurrence" issue template). The PR
that adds the dated record says `Closes #<sub-issue>` and `Refs #<parent>`. The parent's
"Sub-issues progress" column on the board is the completion tracker.

Edits to a living README are ordinary PRs that say `Refs #NN`. They never close anything.

### One-off work whose output keeps changing

Some one-off issues produce a document that will be revised later (a runbook, a
recommendation). Treat them as living documents from the start: the issue closes when the
first version merges, and later revisions are PRs that say `Refs #NN`.

## PR keywords

| Keyword | Effect | Use it when |
|---|---|---|
| `Closes #NN` | merging closes issue NN and moves it to Done | the PR is the completion record, or the dated record for a recurrence sub-issue |
| `Refs #NN` | links the PR to issue NN, nothing closes | editing a living README, or any partial progress |

## Labels

- `kind:` what type of work it is (exactly one per issue)
- `cadence:` one-off, or how often it recurs
- `area:` the shared thing it depends on: website, b200, fair-llm, drone
- `partner:` the outside party that has to show up
- `needs-scope` no definition of done yet; `needs-external-help`; `blocked`; `faculty-lecture`
  (the tool should be presented to faculty at a coaching seminar when it ships)

## Board fields

- **Driver** who owns the line of effort
- **Doc** the path under `loe/` where its documentation lives
- **Priority**, **Target date** set by the driver and the lab lead
