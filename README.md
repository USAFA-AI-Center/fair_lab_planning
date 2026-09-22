<p align="center">
  <img src="assets/FAIR-Logo-2D-Blue.png" alt="FAIR - Falcon AI Research Lab, United States Air Force Academy" width="520">
</p>

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

Run `bin/loe record <issue#>` to generate it with the header filled in, write it, open a PR
whose description says `Closes #NN`. Merge closes the issue.

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
`python3` and an authenticated `gh`. Two commands do the work, and they touch different things.

**Generate the issues for a recurring line of effort** (touches GitHub only, nothing in the repo):

```
bin/loe schedule 9 --start 2026-10 --count 12        # or --until 2027-09
```

One sub-issue per period under #9, each on the board as Todo with its due date as Target date,
labeled `recurrence`, with its record path in the body. Periods that already exist are skipped.

**Write one period's report** (touches one file, on its own branch):

```
bin/loe record 33            # 33 is October's sub-issue; or: bin/loe record 9 --period 2026-10
```

This creates a branch from `origin/main` and writes the record file from #9's template with the
header filled in. Write the report, delete the `unfilled` line, then:

```
bin/loe submit               # commits, pushes, opens the PR with "Closes #33  Refs #9"
```

The check runs, you merge, #33 closes, and the board moves it to Done. A one-off issue works the
same way: `bin/loe record 7`, write, `bin/loe submit`, and the PR says `Closes #7`.

**Without the CLI:** open the issue, follow its record path, create the file on GitHub, and at the
bottom of the editor choose "Create a new branch for this commit and start a pull request." Put
the `Closes` line in the PR description. Same result.

**Only the driver writes the record.** `schedule`, `record`, `submit`, and `template --set` refuse
unless your GitHub account is an assignee of the issue (for a recurrence, of the sub-issue or its
parent), and the PR check fails a PR whose author is not one. The Driver line in the body is
prose; the assignee list is what GitHub can verify, so assign the driver before records start.
Add a second assignee when someone needs to cover for the driver.

Other commands:

```
bin/loe doc <issue#> --set                   write the Doc path into a new issue's body
bin/loe template <issue#> [--set]            show or write the issue's ## Template block
bin/loe check                                lint the docs/ tree
bin/loe check --pr <PR#>                     what the workflow runs on every PR
```

### Templates live in the issues

`docs/` holds data only. The form a record is generated from lives in the issue that owns it,
as a fenced block under a `## Template` heading in the issue body. A record is rendered from,
in order:

1. the `## Template` block in the issue's own body;
2. the block in the issue named by a `**Template:** #N` line, so several lines of effort can
   share one form;
3. the built-in default in `bin/loe` for a recurring or a one-off issue.

`bin/loe template <issue#>` shows the effective template and where it came from.
`bin/loe template <issue#> --set` writes the built-in default into the issue as a `## Template`
block; the driver then edits it there, and every later record follows it. A template is ordinary
markdown with these fields: `{{title}}`, `{{parent}}`, `{{sub_issue}}`, `{{driver}}`, `{{period}}`,
`{{date}}`, `{{issue}}`. Keep an `**Issue:** #{{sub_issue}}` line in it (or `#{{issue}}` for a
one-off); the lint and the PR check rely on that line.

Every generated file carries an `unfilled` marker on its second line. Delete it when the record
is written. `submit` refuses while it is there, and the PR check fails a PR that closes an issue
whose record still carries it. That is what keeps a scaffold from ever closing an issue.

The `check` workflow runs on every pull request. A PR that says `Closes #NN` fails unless it
adds the file at that issue's record path and that file no longer carries the `unfilled` line.

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
