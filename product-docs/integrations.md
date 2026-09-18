# Integrations

Connect TaskFlow to the tools your team already has open, so that task activity reaches people where they are working.

## Available integrations

| Integration | What it does | Who can set it up |
| --- | --- | --- |
| Slack | Posts task activity to a channel, and creates tasks from messages. | Workspace Admin or Owner |
| Google Calendar | Shows tasks with due dates as calendar events. | Any member, for their own calendar |
| Webhooks | Sends a JSON payload to a URL you control when something changes. | Workspace Admin or Owner |

---

## Slack

### Before you begin

- Admin or Owner in the TaskFlow workspace.
- Permission to install apps in your Slack workspace. If your Slack requires approval, the request goes to a Slack admin and the connection stays pending until approved.

### Connect

1. Go to **Settings > Integrations > Slack** and select **Connect**.
2. Sign in to Slack and select **Allow**.
3. Choose the channel that receives notifications. The channel must be public, or TaskFlow must be invited to the private channel first with `/invite @TaskFlow`.
4. Choose which events post to the channel:

   | Event | Posts when | Suggested |
   | --- | --- | --- |
   | Task created | Any task is created | Off for busy workspaces |
   | Task assigned | A task is assigned to someone | On |
   | Task completed | A task moves to done | On |
   | Due date approaching | 24 hours before a due date | On |
   | Comment added | Anyone comments | Off |

5. Select **Save**.

Turning everything on is the fastest way to get a channel muted. Start with assignment and completion, and add more only when someone asks for it.

### Create tasks from Slack

With the integration connected, use `/taskflow` in any channel TaskFlow is in:

```text
/taskflow Draft the Q4 summary --due 2026-10-15 --assign @priya --priority high
```

The task is created in the workspace connected to that channel. Flags are optional; without them you get a task with a title, assigned to nobody.

You can also create a task from an existing message: hover the message, select **More actions**, then **Create TaskFlow task**. The message text becomes the description and a permalink back to the thread is added.

### Verify

Create a test task and confirm it posts. If nothing appears, the usual cause is that TaskFlow is not a member of the channel — reinviting it resolves this without reconnecting the integration.

### Disconnect

**Settings > Integrations > Slack > Disconnect**. Tasks created from Slack remain; the Slack links in them stop resolving for people without access to the original channel.

---

## Google Calendar

Each member connects their own calendar; this is not a workspace-wide setting.

1. Go to **Settings > Integrations > Google Calendar** and select **Connect**.
2. Sign in to Google and grant calendar access.
3. Choose which workspaces to sync, and whether to include tasks assigned to others.
4. Select **Save**.

Tasks with a due date appear as all-day events. Tasks without a due date do not sync, because there is nothing to place them on.

Sync runs one way, from TaskFlow to Google. Editing the event in Google Calendar does not change the task, and the edit is overwritten at the next sync, which runs every 15 minutes. Completed tasks are removed from the calendar at the next sync.

---

## Webhooks

Webhooks are for reacting to changes without polling the API.

### Create a webhook

1. Go to **Settings > Integrations > Webhooks** and select **Add webhook**.
2. Enter an HTTPS endpoint URL. Plain HTTP is rejected.
3. Select the events to subscribe to: `task.created`, `task.updated`, `task.completed`, `task.deleted`.
4. Copy the **signing secret**. It is shown once.
5. Select **Save**.

### Payload

```json
{
  "event": "task.completed",
  "occurred_at": "2026-09-18T11:04:22Z",
  "workspace_id": 7,
  "data": {
    "id": 1024,
    "title": "Write onboarding guide",
    "status": "done",
    "assignee_id": 42
  }
}
```

### Verify the signature

Every request carries `X-TaskFlow-Signature`, an HMAC-SHA256 of the raw request body using your signing secret. Compute the same value and compare before trusting the payload. Compare the raw body, not a re-serialised version of the parsed JSON — key order will differ and the comparison will fail.

### Delivery and retries

A delivery succeeds when your endpoint returns any 2xx status within 5 seconds. Failures retry after 1, 5, 25, and 125 minutes, then stop. After 20 consecutive failures the webhook is disabled and the workspace Owner is emailed.

Deliveries are at-least-once, so the same event can arrive twice. Use `occurred_at` together with the task `id` to discard duplicates. Order is not guaranteed.

Recent deliveries and their responses are listed under the webhook, which is where to look first when something did not arrive.

---

## Next steps

- [Tasks API](../api-docs/sample-api.md) — for reading and writing tasks directly.
- [Team workspaces](team-workspaces.md) — who can manage integrations.
