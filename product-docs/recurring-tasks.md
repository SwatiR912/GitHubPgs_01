# Recurring tasks

Set a task to repeat on a schedule, so that work you do every week does not have to be created every week.

**Time:** about 5 minutes.

## Before you begin

- A task you want to repeat. Recurrence is set on an existing task, not at creation.
- Edit access to the workspace the task is in. In a team workspace, Viewers cannot set recurrence.

## How recurrence works

A recurring task is a **series**: one set of rules that generates **occurrences**. Only one occurrence is open at a time. When you complete the open occurrence, TaskFlow creates the next one according to the schedule.

This matters for two reasons. Your board never fills up with future copies of the same task. And if you do not complete an occurrence, the next one is not created — the series waits.

## Set a repeat schedule

1. Open the task and select **Repeat** in the detail panel.
2. Choose a frequency: **Daily**, **Weekly**, **Monthly**, or **Custom**.
3. Set the details for that frequency:

   | Frequency | What you set | Example |
   | --- | --- | --- |
   | Daily | Every *n* days | Every 2 days |
   | Weekly | Every *n* weeks, and which weekdays | Every week on Monday and Thursday |
   | Monthly | Day of month, or an ordinal weekday | The 1st, or the last Friday |
   | Custom | An interval in days, weeks, or months | Every 10 days |

4. Under **Ends**, choose **Never**, **On a date**, or **After *n* occurrences**.
5. Select **Save**.

A repeat icon appears on the task, and the detail panel shows the schedule in plain language: "Repeats every week on Monday".

## Verify it worked

Complete the current occurrence. Within a few seconds a new task appears in **Open**, with the same title, labels, and assignee, and a due date set by the schedule. If it does not appear, the series has reached its end condition — check **Repeat** on the completed task.

## What carries over, and what does not

| Carries over | Does not carry over |
| --- | --- |
| Title, description, labels | Comments |
| Assignee | Attachments |
| Priority | Subtask completion state |
| Checklist items, unchecked | Time logged |

If you edit the title or labels on an occurrence, the change applies to that occurrence only. To change the whole series, edit the schedule through **Repeat > Edit series**.

## Missed occurrences

If an occurrence's due date passes without completion, TaskFlow marks it **Overdue** and does not create the next one. The series resumes when you complete or skip the overdue occurrence.

To skip without completing, open the task and select **Repeat > Skip this occurrence**. The occurrence is closed as skipped, the next one is created, and your completion statistics are not affected.

This behaviour is deliberate: a weekly report that you missed for a month should leave one overdue task, not four identical ones.

## End or pause a series

- **Pause** — select **Repeat > Pause**. The current occurrence stays open; no new ones are created. Resume from the same menu.
- **End** — select **Repeat > Remove repeat**. The current occurrence becomes an ordinary task. Past occurrences are unaffected.

Deleting a task that is part of a series deletes only that occurrence. To stop the series, remove the repeat first.

## Next steps

- [Team workspaces](team-workspaces.md) — recurring tasks in shared workspaces, and who can edit a series.
- [Integrations](integrations.md) — get a Slack reminder when an occurrence is created.
