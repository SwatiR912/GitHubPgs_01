# TaskFlow API reference

Create, read, update, and complete tasks programmatically.

**Base URL:** `https://api.taskflow.io/v1`
**Format:** JSON request and response bodies. Send `Content-Type: application/json` on requests with a body.

## Authentication

Every request needs a bearer token:

```http
Authorization: Bearer YOUR_TOKEN
```

Create a token in **Settings > API tokens**. Tokens carry the permissions of the account that created them and are scoped to one workspace. A token is shown once; if you lose it, revoke it and create another.

Requests without a valid token return `401 unauthorized`.

---

## POST /tasks

Creates a task in the authenticated user's workspace.

### Request body

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `title` | string | Yes | Task title. Maximum 255 characters. |
| `due_date` | string | No | ISO 8601 date, `YYYY-MM-DD`. Must be today or later. |
| `priority` | string | No | `low`, `medium`, or `high`. Default `medium`. |
| `assignee_id` | integer | No | User ID of the assignee. The user must be a member of the workspace. |
| `labels` | array | No | Label names. Labels that do not exist are created. Maximum 10 per task. |
| `workspace_id` | integer | No | Target workspace. Defaults to the token's workspace. |

### Example request

```bash
curl -X POST https://api.taskflow.io/v1/tasks \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -H 'Content-Type: application/json' \
  -d '{
    "title": "Write onboarding guide",
    "due_date": "2026-10-30",
    "priority": "high",
    "assignee_id": 42,
    "labels": ["Documentation"]
  }'
```

### Example response

`201 Created`

```json
{
  "id": 1024,
  "title": "Write onboarding guide",
  "due_date": "2026-10-30",
  "priority": "high",
  "status": "open",
  "assignee_id": 42,
  "labels": ["Documentation"],
  "workspace_id": 7,
  "created_at": "2026-09-18T10:22:00Z",
  "updated_at": "2026-09-18T10:22:00Z"
}
```

### Response fields

| Field | Type | Description |
| --- | --- | --- |
| `id` | integer | Task identifier. Use it in the paths below. |
| `status` | string | `open`, `in_progress`, or `done`. New tasks are `open`. |
| `created_at`, `updated_at` | string | ISO 8601 timestamps in UTC. |

---

## GET /tasks

Lists tasks in the workspace, newest first.

### Query parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `status` | string | all | Filter by `open`, `in_progress`, or `done`. |
| `assignee_id` | integer | all | Filter by assignee. Pass `unassigned` for tasks with no assignee. |
| `due_before` | string | none | ISO 8601 date. Returns tasks due on or before this date. |
| `label` | string | all | Filter by label name. Repeat the parameter to match several labels. |
| `limit` | integer | 50 | Results per page, 1 to 200. |
| `cursor` | string | none | Pagination cursor from the previous response. |

```bash
curl 'https://api.taskflow.io/v1/tasks?status=open&due_before=2026-10-01&limit=20' -H 'Authorization: Bearer YOUR_TOKEN'
```

`200 OK`

```json
{
  "data": [ { "id": 1024, "title": "Write onboarding guide", "status": "open" } ],
  "next_cursor": "c3RhcnQ6MTAyNA",
  "has_more": true
}
```

When `has_more` is `true`, pass `next_cursor` as `cursor` to fetch the following page. Cursors expire after 24 hours.

---

## GET /tasks/{id}

Returns one task. Responds `404 not_found` if the task does not exist or belongs to a workspace your token cannot read — the two cases are deliberately indistinguishable, so that identifiers cannot be probed.

---

## PATCH /tasks/{id}

Updates the fields you send and leaves the rest untouched. Accepts the same fields as `POST /tasks`.

```bash
curl -X PATCH https://api.taskflow.io/v1/tasks/1024 -H 'Authorization: Bearer YOUR_TOKEN' -H 'Content-Type: application/json' -d '{"status": "in_progress"}'
```

To clear an optional field, send it as `null`. Omitting it leaves the current value in place.

---

## DELETE /tasks/{id}

Deletes a task. Responds `204 No Content` with an empty body.

Deletion is permanent — there is no trash and no undo. To keep a record, set `status` to `done` instead.

---

## Errors

| Status | Code | Meaning | What to do |
| --- | --- | --- | --- |
| 400 | `invalid_field` | A required field is missing or malformed. | Fix the request. Retrying unchanged will not help. |
| 401 | `unauthorized` | Token is missing, malformed, revoked, or expired. | Check the header format, then the token's status in Settings. |
| 403 | `forbidden` | The token's account cannot act on this workspace. | Ask a workspace admin for access. |
| 404 | `not_found` | No such task, or not visible to this token. | Check the ID and the workspace. |
| 409 | `conflict` | The task was modified by someone else since you read it. | Re-read the task and reapply your change. |
| 422 | `validation_error` | The request is well formed but the values are not allowed, for example a due date in the past. | Read `message`; it names the field. |
| 429 | `rate_limited` | Too many requests. | Back off using `Retry-After`. |
| 500 | `server_error` | Something failed on our side. | Retry with backoff. If it persists, contact support with `request_id`. |

All errors share one shape:

```json
{
  "error": {
    "code": "validation_error",
    "message": "due_date must be today or a future date.",
    "field": "due_date",
    "request_id": "req_8f3a21c7"
  }
}
```

Quote `request_id` when contacting support; it is how a request is found in the logs.

---

## Rate limits

600 requests per minute per token. Every response reports the current state:

```http
X-RateLimit-Limit: 600
X-RateLimit-Remaining: 574
X-RateLimit-Reset: 1758106800
```

On a `429`, wait the number of seconds in `Retry-After` before retrying. Retry `429`, `500`, and `503` with exponential backoff starting at one second; do not retry `400`, `401`, `403`, or `422`, which will fail identically.

---

## Related

- [Getting started with TaskFlow](../product-docs/getting-started.md)
- [Integrations](../product-docs/integrations.md) — webhooks, for reacting to task changes instead of polling this API.
