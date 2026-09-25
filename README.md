<p align="center">
  <img src="assets/FAIR-Logo-2D-Blue.png" alt="FAIR - Falcon AI Research Lab, United States Air Force Academy" width="520">
</p>

# FAIR Lab planning

This repository provides automations for creating/documenting tasks for the Falcon AI Research Lab (FAIR). 

Source of truth for all active tasks for the current year: [FAIR Lab Lines of Effort 2026](https://github.com/orgs/USAFA-AI-Center/projects/3).

All work tracking will be contained within the `docs/` directory. 

## How we complete and track work

Everything happens on GitHub, in issue and PR comments. A bot (the `logbook` GitHub App) does the
rest. Issues are NEVER closed by hand or through
[the project board](https://github.com/orgs/USAFA-AI-Center/projects/3): the bot closes them by
merging the PR that adds the issue's record to `docs/`, so every Done has a write-up behind it.

Only three things are ever done by a person:

1. **Write the issue.** Give it a `cadence:` label and assign the driver.
2. **Write the record** in the PR the bot opens.
3. **Comment `/done`** to hand each step back to the bot.

### One-off work (`cadence:once`)

1. Write the issue. The bot adds its record path, puts it on the board as Todo, and comments
   with the next step.
2. When the work is finished, comment `/done` on the issue. The bot opens a PR with the record
   file generated from the issue's template and replies with a link to edit it.
3. Write the record: click the link, or pull the branch, edit, and push.
4. Comment `/done` on the PR. The bot checks the record, merges it, the issue closes, and the
   board moves it to Done.

The record is one file named after the issue's exact title, spaces as underscores:

```
docs/Make_FAIR_Lab_logo.md
```

### Recurring work (`cadence:weekly` / `monthly` / `quarterly` / `semester` / `annual`)

The parent issue never closes. Each period is a sub-issue, and each sub-issue closes with its
own dated record.

1. Write the parent issue. The bot adds its record folder, puts it on the board as Recurring,
   and comments with the next step.
2. Comment `/schedule 2026-10 2027-05` on the parent. The bot creates one sub-issue per period,
   each on the board as Todo with its due date as the Target date. Run it again with a later end
   to extend; existing periods are skipped.
3. When a period's work is finished, comment `/done` on that period's sub-issue (or
   `/done 2026-10` on the parent). Only that period gets a PR.
4. Write the record, then comment `/done` on the PR, as above.

```
docs/Set_up_and_manage_monthly_Faculty_Coaching_Seminars/
  2026-10.md       one dated record per period
  2026-11.md
```

The parent's "Sub-issues progress" column on the board is the completion tracker.

Period formats: weekly `2026-W40`, monthly `2026-10`, quarterly `2026-Q4`, semester
`2026-fall` / `2027-spring`, annual `2026`.

### Commands

Only an assignee of the issue (for a sub-issue, of it or its parent) can run these. Add a second
assignee when someone needs to cover for the driver. The bot reacts with 👀 when it sees a
command and replies with the result.

| Where | Comment | What happens |
|---|---|---|
| recurring issue | `/schedule <first> [<last>]` | one sub-issue per period |
| one-off issue or sub-issue | `/done` | the work is finished: the bot opens the record PR and links it |
| recurring issue | `/done <period>` | the same, for that period's sub-issue |
| any issue | `/template` | copies the record template into the issue body to edit |
| record PR | `/done` | the record is written: the bot checks it and merges it |
| anywhere | `/help` | lists the commands |

### Templates live in the issues

`docs/` holds records only. The form a record is generated from lives in the issue that owns it,
as a fenced block under a `## Template` heading in the issue body. A record is rendered from, in
order:

1. the `## Template` block in the issue's own body (for a sub-issue, its parent's);
2. the block in the issue named by a `**Template:** #N` line, so several lines of effort can
   share one form;
3. the built-in default for a recurring or a one-off issue.

Comment `/template` to copy the effective template into the issue, then edit it there; every
record opened after that follows it. Fields: `{{title}}`, `{{parent}}`, `{{sub_issue}}`,
`{{driver}}`, `{{period}}`, `{{date}}`, `{{issue}}`. Keep the `**Issue:** #{{sub_issue}}` line
(or `#{{issue}}` for a one-off); the check relies on it.

### The check

The `check` workflow runs on every PR, and `main` only accepts a PR that passes it. A PR that
says `Closes #NN` fails unless it adds that issue's record, the record is no longer the
unwritten scaffold the bot generated, and it keeps its `**Issue:** #NN` line.

## For maintainers

`bin/logbook` is the bot: `.github/workflows/bot.yml` runs `logbook handle` on issue, comment,
and merge events with the App's token. It needs `python3` (standard library only) and `gh`.
The same work runs locally as yourself:

```
bin/logbook schedule <issue#> --start <period> (--count N | --until <period>)
bin/logbook template <issue#> [--set [--from FILE]]
bin/logbook doc <issue#> [--set]                 print or write an issue's record path
bin/logbook check [--pr <PR#>]                   lint docs/; with --pr, what the check runs
```

One-time setup: the `logbook` GitHub App (Contents, Issues, Pull requests: read/write;
Metadata: read; organization Projects: read/write), installed on this repo, with its ID in the
repo variable `LOGBOOK_APP_ID` and its private key in the repo secret `LOGBOOK_APP_KEY`.

## Labels

- `kind:` what type of work it is (exactly one per issue)
- `cadence:` one-off, or how often it recurs (weekly, monthly, quarterly, semester, annual)
- `area:` the shared thing it depends on: website, b200, fair-llm, drone
- `partner:` the outside party that has to show up
- `needs-scope` no definition of done yet; `needs-external-help`; `blocked`; `faculty-lecture`
  (the tool should be presented to faculty at a coaching seminar when it ships);
  `recurrence` (one period of a recurring line, created by `/schedule`)

## Board fields

- **Driver** who owns the line of effort
- **Doc** the path under `docs/` where its records live (the bot writes it)
- **Priority**, **Target date** set by the driver and the lab lead
