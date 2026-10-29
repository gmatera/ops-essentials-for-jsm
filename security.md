---
title: "Security: Ops Essentials for Jira Service Management"
description: How Ops Essentials for Jira Service Management protects your data, the permissions it uses and how to report a vulnerability.
---

**Reporting a vulnerability.** Write to **support@steadyseam.com** with the details; please do not disclose it
publicly before we have answered. We acknowledge reports within two business days and keep you informed until
the issue is fixed.

**How the app is built**

- It runs on Atlassian Forge: no servers of ours. Data is kept in your site's Forge storage (Forge key-value
  store and Forge SQL), hosted by Atlassian. Steadyseam does not receive or see your data.
- The app's page is in Jira administration, available only to Jira administrators.
- **Permissions.** `storage:app`, for the settings and the alert archive. `read:ops-alert`, `write:ops-alert`
  and `read:ops-config` of Jira Service Management, with impersonation, to open and close heartbeat alerts and
  to read teams and alerts. Atlassian's operations API does not accept calls made by an app on its own, so the
  app acts on behalf of the administrator who set up the connection.
- **External domains**, used only for the stakeholder channels you configure: `hooks.slack.com`,
  `chat.googleapis.com`, `*.webhook.office.com`, `*.logic.azure.com`, `*.environment.api.powerplatform.com`,
  `api.sendgrid.com`, `api.resend.com`. Because of these optional channels the app is not eligible for the
  "Runs on Atlassian" badge.
- **Heartbeat addresses** are protected by random tokens of 32 characters; a token can be regenerated at any
  time. **CSV exports** use one-time links, valid for 10 minutes.
- **Webhook addresses and API keys** are encrypted in Forge's secret storage and never shown again after they
  are saved.
- **Logs** contain counts, errors and the structure of responses, not the content of alerts.
- No usage analytics, no tracking, no data sent to Steadyseam.

See also the [privacy policy](privacy-policy.md) and the [Data Processing Addendum](dpa.md).
