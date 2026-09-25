<p align="center">
  <img src="assets/FAIR-Logo-2D-Blue.png" alt="FAIR - Falcon AI Research Lab, United States Air Force Academy" width="520">
</p>

# FAIR Lab planning

This repository provides automations for creating/documenting tasks for the Falcon AI Research Lab (FAIR). 

Source of truth for all active tasks for the current year: [FAIR Lab Lines of Effort 2026](https://github.com/orgs/USAFA-AI-Center/projects/3).

All work tracking will be contained within the `docs/` directory. 

## Pick your task

1. [Recurring task](#recurring-task): repeats on a schedule, weekly to annual.
2. [One-off task](#one-off-task): done once, then closed.
3. [Standing task](#standing-task): an ongoing responsibility, recorded one change at a time.

## How it works

A bot, `fair-logbook`, runs everything from issue and PR comments. Never close an issue by hand.
The bot closes it when it merges the issue's record into `docs/`. A record is the write-up of the
finished work: a markdown file generated from the issue's template.

Only the issue's assignees can run commands. The assignee is the driver. On a sub-issue, the
parent's assignees count too. To let someone cover for the driver, add them as a second assignee.

## Recurring task

For work that repeats: `weekly`, `monthly`, `quarterly`, `semester`, `annual`. The parent issue
stays open. Each period is a sub-issue with its own record.

1. Open **New issue → Line of effort**.
2. Set the title. It becomes the record folder, so settle it now.
3. Pick **Cadence** (`weekly` to `annual`) and **Kind**.
4. Set the driver under **Assignees** in the sidebar. Add `area:`, `partner:`, and status labels there if they apply.
5. Click **Create**. The bot labels the issue, puts it on the board as Recurring, and comments the next steps.
6. Comment `/template`. The bot fills in the body.
7. Edit the body: **Notes**, **Done means**, and the record template under `## Template`. Every period's record is generated from it.
8. Comment `/schedule <start date> <how many>`, e.g. `/schedule 2026-10-01 8` for eight months from October. The bot creates one sub-issue per period, each on the board with its due date. Re-run to extend; existing periods are skipped.
   Examples for every cadence: [Schedule formats](#schedule-formats).
9. When a period's work is finished, comment `/done` on **that period's sub-issue**. The bot opens a PR and replies with an edit link.
10. Click the edit link, replace the placeholder text, click **Commit changes**.
11. Comment `/done` on the PR. The bot merges it. The sub-issue closes and the board marks it Done.
12. Repeat 9 to 11 every period.
13. When the line of effort ends and every sub-issue is closed, comment `/done` on the parent. The bot opens a closeout PR. Fill it in and comment `/done` on the PR. The parent closes.

`/done` on the parent while any sub-issue is open is refused, with the list of open sub-issues.

The bot keeps a **Sub-issues** list in the parent's body: each period, whether it is open or done,
and a link to its record. Don't edit it; it is rewritten on every `/schedule` and every merge.

Deleting the parent closes its open sub-issues as not planned, removes them from the board, and
closes their open record PRs.

Records: `docs/Set_up_and_manage_monthly_Faculty_Coaching_Seminars/2026-10.md`, one per period,
and `Closeout.md` at the end.

## One-off task

For work done once: `once`. The issue closes when its record merges.

1. Open **New issue → Line of effort**.
2. Set the title. It becomes the record file name, so settle it now.
3. Pick **Cadence** `once` and **Kind**.
4. Set the driver under **Assignees** in the sidebar. Add `area:`, `partner:`, and status labels there if they apply.
5. Click **Create**. The bot labels the issue, puts it on the board as Todo, and comments the next steps.
6. Comment `/template`. The bot fills in the body.
7. Edit the body: **Notes**, **Done means**, and the record template under `## Template`.
8. Do the work. If it turns up new work, comment `/followup <title>`, e.g. `/followup Add a dark-mode logo`. The bot files it as a new one-off issue and lists it under **Follow-ups** in this issue.
9. When the work is finished, comment `/done` on the issue. The bot opens a PR and replies with an edit link. The record's **Follow-ups filed** section is already filled in.
10. Click the edit link, replace the placeholder text, click **Commit changes**.
11. Comment `/done` on the PR. The bot merges it. The issue closes and the board marks it Done.

Record: `docs/Make_FAIR_Lab_logo.md`.

## Standing task

For an ongoing responsibility with no schedule: `standing`. The parent issue stays open. Each
discrete change (a rebuild, an incident, a move) is a sub-issue with its own record.

1. Open **New issue → Line of effort**.
2. Set the title. It becomes the record folder, so settle it now.
3. Pick **Cadence** `standing` and **Kind**.
4. Set the driver under **Assignees** in the sidebar. Add `area:`, `partner:`, and status labels there if they apply.
5. Click **Create**. The bot labels the issue, puts it on the board as Recurring, and comments the next steps.
6. Comment `/template`. The bot fills in the body.
7. Edit the body: **Notes**, **Done means**, and the record template under `## Template`. Every change's record is generated from it.
8. When a change happens, comment `/change GPU rebuild` on the parent. The bot files the sub-issue.
9. When the change is done, comment `/done` on **that sub-issue**. The bot opens a PR and replies with an edit link.
10. Click the edit link, replace the placeholder text, click **Commit changes**.
11. Comment `/done` on the PR. The bot merges it. The sub-issue closes and the board marks it Done.
12. Repeat 8 to 11 for every change.
13. When the responsibility ends and every sub-issue is closed, comment `/done` on the parent. The bot opens a closeout PR. Fill it in and comment `/done` on the PR. The parent closes.

The bot keeps a **Sub-issues** list in the parent's body, rewritten on every `/change` and every merge.

Records: `docs/B200_Management/2026-09-25_GPU_rebuild.md`, one per change, and `Closeout.md` at
the end.

## Schedule formats

`/schedule <start date> <how many>`. The start date is any day: the bot starts at the week, month,
quarter, semester, or year that contains it. A period is one unit of the issue's cadence.

| Cadence | Example | Creates | Sub-issue titles | Each period is due |
|---|---|---|---|---|
| `weekly` | `/schedule 2026-09-28 5` | 5 weeks, Sep 28 to Nov 1 2026 | Week of Sep 28, 2026 | Sunday |
| `monthly` | `/schedule 2026-10-01 8` | 8 months, Oct 2026 to May 2027 | October 2026 | last day of the month |
| `quarterly` | `/schedule 2026-10-01 4` | 4 quarters, Oct 2026 to Sep 2027 | Q4 2026 (Oct to Dec) | last day of the quarter |
| `semester` | `/schedule 2026-08-01 4` | 4 semesters, fall 2026 to spring 2028 | Fall 2026 | Dec 15 (fall), May 15 (spring) |
| `annual` | `/schedule 2026-01-01 3` | 3 years, 2026 to 2028 | 2026 | Dec 31 |

`next` instead of a date starts at the next period of the issue's cadence: `/schedule next 5` is
the next 5 weeks on a weekly issue, the next 5 months on a monthly one.

Quarters are calendar quarters (Q1 is Jan to Mar). Semesters are fall (Jul to Dec) and spring
(Jan to Jun). Record files use a short, sortable name: `2026-09-28.md` (the week's Monday),
`2026-10.md`, `2026-Q4.md`, `2026-fall.md`, `2026.md`.

## Commands

| Where | Comment | Does |
|---|---|---|
| any issue but a sub-issue | `/template` | fills in the body and the record template |
| recurring issue | `/schedule <start date> <how many>` | creates the sub-issues; re-run to extend |
| standing issue | `/change <short name>` | files one change as a sub-issue |
| one-off issue | `/followup <title>` | files new work as its own issue; the record lists it |
| one-off issue or sub-issue | `/done` | opens the record PR |
| recurring or standing issue | `/done` | once every sub-issue is closed, opens the closeout PR |
| record PR | `/done` | checks the record and merges it |
| anywhere | `/help` | lists the commands |

## Labels

| Label | Required | Set by | Options |
|---|---|---|---|
| `cadence:` | yes, one | the form's **Cadence** | `once`, `weekly`, `monthly`, `quarterly`, `semester`, `annual`, `standing` |
| `kind:` | yes, one | the form's **Kind** | `admin`, `build`, `infra`, `integration`, `report`, `service` |
| `area:` | if it applies | sidebar | `b200`, `drone`, `fair-llm`, `website` |
| `partner:` | if it applies | sidebar | the outside party, e.g. `partner:afit` |
| status | if it applies | sidebar | `needs-scope`, `needs-external-help`, `blocked`, `faculty-lecture` (present the tool at a coaching seminar when it ships) |
| `recurrence` | | the bot | marks a sub-issue made by `/schedule` or `/change` |

## Templates

A record is generated from the `## Template` block in the issue body, added by `/template`. A
sub-issue uses its parent's. `**Template:** #N` in the body borrows another issue's template. With
neither, the bot uses the default. Leave the `**Issue:**` line and the `{{...}}` fields as they are.

## The check

Every PR runs `check`. A PR that closes an issue must add that issue's record, written, with its
`**Issue:** #N` line. `main` accepts nothing else.

## Board fields

- **Assignees** the driver
- **Doc** where its records live; the bot writes it
- **Priority**, **Target date** set by the driver and the lab lead

## Maintainers

`bin/logbook` is the bot. `.github/workflows/bot.yml` runs it with the App's token. It also runs
locally as you:

```
bin/logbook schedule <issue#> --start <date|period> (--count N | --until <date|period>)
bin/logbook template <issue#> [--set]
bin/logbook doc <issue#> [--set]
bin/logbook check [--pr <PR#>]
```

The App needs Contents, Issues, and Pull requests (read/write), Metadata (read), and organization
Projects (read/write). Its ID is in the variable `LOGBOOK_APP_ID`, its key in the secret `LOGBOOK_APP_KEY`.
