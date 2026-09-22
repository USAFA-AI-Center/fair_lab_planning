# FAIR Lab planning

Issues in this repo are the FAIR Lab lines of effort. The board is the org project
[FAIR Lab Lines of Effort 2026](https://github.com/orgs/USAFA-AI-Center/projects/3).
The `docs/` directory holds the documentation that proves each line of effort was done.

## How an issue gets to Done

Nobody closes an issue by hand. A pull request closes it, and the pull request adds the
documentation. Merging the PR closes the issue and the board moves it to Done on its own.
That is the whole mechanism: no PR, no Done, and every Done has a write-up behind it.

There are two kinds of documentation, and the issue's `cadence:` label tells you which one
applies.

### One-off work (`cadence:once`): a completion record

One markdown file, written once, at the path in the issue's **Doc** line. The path is the
issue's exact title with spaces as underscores (path-unsafe characters dropped):

```
docs/Make_FAIR_Lab_logo.md
```

Copy `docs/TEMPLATE_completion_record.md`, fill it in, open a PR whose description says
`Closes #NN`. Merge closes the issue.

### Recurring work and standing responsibilities: a living document

The parent issue never closes. Its documentation is a folder named the same way:

```
docs/Set_up_and_manage_monthly_Faculty_Coaching_Seminars/
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

## Tools: `bin/loe`

The issues are the source of truth; the scripts read them and generate the rest. Needs
`python3` and an authenticated `gh`.

```
bin/loe record <issue#>                      write the completion record for a one-off issue
bin/loe record <issue#> --period 2026-10     write the dated record for a recurring issue
                                             (finds or creates the recurrence sub-issue)
bin/loe schedule <issue#> --start 2026-10 --count 12
                                             create 12 recurrence sub-issues with due dates
bin/loe doc <issue#> --set                   write the Doc path into a new issue's body
bin/loe check                                lint the docs/ tree
bin/loe check --pr <PR#>                     verify the PR documents every issue it closes
```

Every file comes out with its fields already filled from the issue: number, title, driver,
cycle, date. The command prints the `Closes` / `Refs` lines to paste into the PR.

`schedule` derives the periods from the issue's `cadence:` label (`2026-W40`, `2026-10`,
`2026-Q4`, `2026-fall`, `2026`), makes each sub-issue a child of the parent, puts it on the board
as Todo with its due date as Target date, and labels it `recurrence`. Give it `--count N` or
`--until <period>`. Periods that already have a sub-issue are skipped, so it is safe to re-run.
With `--files` it also writes `docs/<Title>/<period>.md` for every period, so the whole
reporting structure for a line of effort lands in one PR.

### Per-issue templates

Each record is rendered from a template, chosen in this order:

1. the file named by a `**Template:**` line in the issue body, if present;
2. `docs/<Title>/TEMPLATE.md`, if the folder has one;
3. the generic `docs/TEMPLATE_record.md` (or `docs/TEMPLATE_completion_record.md` for a one-off).

So a driver who wants a specific data-gathering form writes it once as `TEMPLATE.md` in their
folder, and every generated record uses it. A template is ordinary markdown with these fields:
`{{title}}`, `{{parent}}`, `{{sub_issue}}`, `{{driver}}`, `{{period}}`, `{{date}}`, `{{issue}}`.
Keep an `**Issue:** #{{sub_issue}}` line in it; the lint and the PR check rely on that line.

Every generated file carries an `unfilled` marker on its second line. Delete it when the record
is written. The lint fails if an issue is closed while its record still carries the marker, which
catches "closed the issue, never wrote the report."

The `check` workflow runs on every pull request. A PR that says `Closes #NN` without adding
the file at that issue's Doc path fails the check.

## Labels

- `kind:` what type of work it is (exactly one per issue)
- `cadence:` one-off, or how often it recurs (weekly, monthly, quarterly, semester, annual)
- `area:` the shared thing it depends on: website, b200, fair-llm, drone
- `partner:` the outside party that has to show up
- `needs-scope` no definition of done yet; `needs-external-help`; `blocked`; `faculty-lecture`
  (the tool should be presented to faculty at a coaching seminar when it ships);
  `recurrence` (one cycle of a recurring line, generated by `bin/loe schedule`)

## Board fields

- **Driver** who owns the line of effort
- **Doc** the path under `docs/` where its documentation lives
- **Priority**, **Target date** set by the driver and the lab lead
