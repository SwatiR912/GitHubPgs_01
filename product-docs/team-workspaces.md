# Team workspaces

Share tasks with the people you work with, control who can change what, and keep personal work separate from team work.

**Time:** about 10 minutes to create a workspace and invite your first members.

## Before you begin

- A TaskFlow account on any plan. Free accounts can belong to team workspaces but cannot create them; creating one requires Team or Business.
- The email addresses of the people you want to invite.

## Personal and team workspaces

**My Workspace** is yours. Nobody else can see it, and it cannot be shared — there is no way to convert a personal workspace into a team one. If work in your personal workspace needs to become shared work, move the tasks (see below).

A **team workspace** is shared. Every member sees every task in it, subject to their role. You can belong to as many team workspaces as you like, and they appear under **My Workspace** in the sidebar.

## Create a team workspace

1. In the sidebar, select **+ New workspace**.
2. Enter a name. Use something a newcomer would recognise — "Marketing", not "MKT-2026-v2".
3. Optionally add a description. It appears on the invitation, which is the moment it is most useful.
4. Select **Create**.

You are the workspace Owner.

## Roles and permissions

Every member has exactly one role per workspace.

| Action | Viewer | Member | Admin | Owner |
| --- | --- | --- | --- | --- |
| See tasks and comments | Yes | Yes | Yes | Yes |
| Comment | Yes | Yes | Yes | Yes |
| Create and edit tasks | No | Yes | Yes | Yes |
| Assign tasks to others | No | Yes | Yes | Yes |
| Set and edit recurrence | No | Yes | Yes | Yes |
| Delete any task | No | Own only | Yes | Yes |
| Invite and remove members | No | No | Yes | Yes |
| Change roles | No | No | Members only | Yes |
| Manage integrations | No | No | Yes | Yes |
| Delete the workspace | No | No | No | Yes |
| Transfer ownership | No | No | No | Yes |

A workspace has exactly one Owner. Transferring ownership makes you an Admin.

**Choosing a role.** Give Member to anyone who does the work, Admin to whoever manages access, and Viewer to stakeholders who need visibility without edit rights. Viewer is also the right role for a client.

## Invite people

1. Open the workspace and select **Members** in the sidebar.
2. Select **Invite**.
3. Enter email addresses, one per line. Up to 25 per invitation.
4. Choose the role they will receive. One role applies to everyone in this invitation; change individuals afterwards.
5. Optionally add a message. It appears in the invitation email.
6. Select **Send invitations**.

Invitations expire after 7 days. An invited person who does not have a TaskFlow account creates one during acceptance and lands directly in the workspace.

### Verify

Open **Members**. Each invited person appears with the status **Invited** until they accept, then **Active**. If someone reports not receiving the invitation, resend it from the row menu rather than sending a second invitation, which creates a duplicate entry.

## Move existing tasks into a workspace

1. In the source workspace, select the tasks. Shift-click selects a range.
2. Select **Move** in the toolbar.
3. Choose the destination workspace and select **Move tasks**.

Comments, attachments, and history move with the task. **Assignees do not.** If the assignee is not a member of the destination workspace, the task arrives unassigned, and TaskFlow does not warn you. Check assignments after a bulk move.

Labels are per workspace. A label that does not exist in the destination is created there.

## Remove someone

1. Open **Members** and select **Remove** from the person's row menu.
2. Choose what happens to their assigned tasks: **Unassign**, or **Reassign to** a specific member.

Removal takes effect immediately. Their comments and history remain, attributed to them. If they were the last Admin, transfer that role first — TaskFlow blocks the removal otherwise.

## Next steps

- [Recurring tasks](recurring-tasks.md) — repeat schedules in shared workspaces.
- [Integrations](integrations.md) — send workspace activity to Slack.
- [Tasks API](../api-docs/sample-api.md) — the `workspace_id` parameter, for creating tasks in a specific workspace.
