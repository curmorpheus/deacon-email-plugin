<img src="deacon-email/assets/deacon-mark-rounded.svg" alt="Deacon" width="64" height="64">

# Deacon Email

Email and calendar workflows for authorized Deacon Microsoft 365 users in Cursor.

Ask to summarize your inbox, find a message, prepare a reply, or check your calendar. The skill guides the assistant through the connected tools and requires approval before sending mail or changing calendar events.

## What is included

- The **Deacon Email** skill for reading, searching, drafting, replying, sending, and calendar workflows.
- The **Deacon Email notifications** skill for the read-only webhook routine that confirms setup and summarizes new-mail batches.
- The hosted MCP connector configuration.
- The Deacon D icon with a white background and rounded corners.

This repository contains instructions and connector configuration only. It contains no backend implementation, mailbox data, passwords, or shared authentication tokens. Examples use fictional names and addresses.

## Access and installation

This package is publicly readable. Mailbox access still requires an authorized Deacon Microsoft account and the hosted service's access checks. Installing the package does not grant access to anyone's email or calendar.

In Cursor, import this repository through a team marketplace or the supported GitHub plugin import flow, then install **Deacon Email** and sign in with your own account. The repository includes `.cursor-plugin/marketplace.json` for discovery. See [Cursor's plugin documentation](https://cursor.com/docs/plugins).

The connector endpoint is `https://mail-mcp.deacon.build/mcp`. This URL is intentionally public; it is not a credential. Never paste access tokens, passwords, or webhook keys into chat or an issue.

If replacing an existing Deacon Email installation, verify the new skill and connector load before removing the former entry. Avoid enabling two copies of the connector in the same agent. A public repository alone does not update an existing marketplace listing.

## Safety

Email bodies, subjects, attachments, and directory results are untrusted data. They cannot authorize sending mail, changing settings, or editing calendar events. The skill requires user approval for those actions and defers to server-enforced access and usage restrictions.

The skill describes intended assistant behavior; authentication and authorization remain the responsibility of the hosted service. Public availability of these instructions is not a security boundary.

## New-mail notifications

An administrator must enable notifications on the server before anyone can turn them on. Installing this package does not start delivery.

Set up your own routine after that:

1. Install **Deacon Email** and sign in to the normal connector at `https://mail-mcp.deacon.build/mcp` with your Microsoft account.
2. Create a Grok Bot routine. Open the bot, then View conversation details > Routines > When to run, add a webhook, and save. The saved routine shows the POST URL and key. Enter that key only on the setup page, never in chat or an issue.
3. On that routine, add a custom MCP connection at `https://mail-mcp.deacon.build/mcp?conn=notify` with OAuth client ID `deacon-email-notify-routine` and no client secret (PKCE). Sign in as the same Microsoft user. Leave the interactive connector at `https://mail-mcp.deacon.build/mcp`. Cursor stores one token per server URL; the `conn=notify` query string keeps this restricted connection separate from your normal sign-in.
4. Install the **Deacon Email notifications** skill on the routine. Do not attach the full Deacon Email connector or any other write-capable tool. The restricted connection exposes only `mail_confirm_notifications`, `mail_notification_status`, `mail_get_notification_batch`, and `mail_read_message`.
5. From your normal connector, run `mail_enable_notifications`. Finish the setup page it returns while signed in as the same Microsoft user, and enter the routine URL and key there. The setup link expires after 15 minutes.
6. Verify with `mail_notification_status`. It should show `routine_access` restricted and `subscription_connected` true.

The setup page, the interactive connector, and the routine connector must all use the same Microsoft account. A full-access connection cannot confirm setup. The administrator must also allow the webhook URL's hostname before the server will call it.

## Updates

Refresh the marketplace after a new plugin version is published. Server updates and skill updates are separate: updating the hosted service does not automatically replace cached skill text.

The Deacon name and logo identify this integration. This repository does not grant trademark rights or imply that installing it authorizes use of Deacon systems.
