---
title: "Privacy Policy: Ops Essentials for Jira Service Management"
description: What data Ops Essentials for Jira Service Management processes, why, where it is kept and for how long.
---

*Last updated: 29 October 2026.*

This policy explains what data the Atlassian Marketplace app **Ops Essentials for Jira Service Management**
(the "App") processes, why, where it is kept and for how long. The App is provided under the Steadyseam brand
by **Sasso di Matera Giuseppe S.a.s.** (Via Clefi 2, 20146 Milano (MI), Italy, VAT IT13043470965), "we" in this
policy. Contact: **support@steadyseam.com**.

## 1. How the App works

The App adds heartbeat monitoring, operations reports, stakeholder updates and an alert archive to Jira
Service Management Cloud. It runs on Atlassian's Forge platform and has no servers of its own: we do not
receive or see the data it stores. Data leaves Atlassian only if your organization configures a stakeholder
channel (section 3).

## 2. Data processed by the App

| Data | Source | Purpose |
|---|---|---|
| Heartbeat settings: name, description, interval, grace period, priority, team | your administrators | Heartbeats |
| Time and optional text of the last call to each heartbeat (up to 500 characters) | your jobs | Shown in the App and in alerts |
| Copies of alerts: number, message, status, priority, owner and responders, source, integration, alias, entity, tags, occurrences, times of creation, acknowledgement and closing | Jira Service Management | Archive, reports, stakeholder updates |
| Stakeholder channels and rules, including e-mail recipients and sender address; webhook addresses and API keys, stored encrypted | your administrators | Stakeholder updates |
| Log of the updates sent and their result | the App | "Recent updates" |
| Atlassian account ID of the administrator who set up the connection | Jira | Background checks act on their behalf |

## 3. Where the data is stored and who receives it

All the data is kept only in your site's Forge storage (Forge key-value store and Forge SQL), hosted by
Atlassian. The App does not send data to us. Atlassian is our only sub-processor (see the
[Atlassian Privacy Policy](https://www.atlassian.com/legal/privacy-policy)).

**Stakeholder channels.** If your organization configures a stakeholder channel, the App sends the alert
number, text, priority, owner, duration and link to the webhook (Slack, Microsoft Teams, Google Chat) or
e-mail service (SendGrid, Resend) chosen and configured by your organization, with its own credentials. These
services are chosen and contracted by your organization, and their processing is governed by its agreements
with them.

## 4. Retention

- Archived alerts: for the number of months set in the App's settings (0 keeps them forever). Older alerts
  are deleted once a day, together with the log of the updates sent about them.
- Heartbeats: only the last call is kept for each heartbeat.
- Settings, heartbeats, channels and rules: until an administrator deletes them.
- When the App is uninstalled, Atlassian deletes the data stored by the App according to its data retention
  policy for Forge apps.

## 5. Logs and tracking

The App's technical logs contain counts, errors and the structure of responses, not the content of alerts.
Atlassian lets us see them only within the limits it sets. The App uses no usage analytics and no tracking, and
sends no data to Steadyseam.

## 6. Roles and your rights

For the data listed in section 2, your organization is the controller and we act as a processor on its
behalf, under our [Data Processing Addendum](dpa.md). Requests from people whose data appears in alerts or
stakeholder settings should be addressed to your organization, which can change or delete the data in Jira
Service Management and in the App; we will assist on request.

When you contact our support, we use your name, e-mail address and the content of your message only to
answer you, and we keep them for up to 24 months after the last exchange. You can ask us to access, correct
or delete them, or object to their use, by writing to support@steadyseam.com. You can also lodge a complaint
with your data protection authority (in Italy, the Garante per la protezione dei dati personali).

## 7. Security

The App's page is available only to Jira administrators. Heartbeat addresses are protected by random tokens,
CSV exports by one-time links valid for 10 minutes, and webhook addresses and API keys are stored encrypted.
See the [security page](security.md).

## 8. Changes

We will publish any change to this policy on this page, with the date of the last update.
