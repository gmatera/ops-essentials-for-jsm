---
title: "Ops Essentials for Jira Service Management: user guide"
description: Install Ops Essentials, set up heartbeats, read operations reports, send stakeholder updates and search the alert archive.
---

Ops Essentials adds heartbeat monitoring, operations reports, stakeholder updates and a long-term alert archive
to Jira Service Management Cloud. Everything is set up on one page in Jira: **Settings > Apps > Ops
Essentials**, with six tabs: Overview, Heartbeats, Reports, Archive, Stakeholders and Settings.

## Requirements

- Jira Service Management Cloud, Standard or Premium plan, with **Operations turned on for at least one team**.
- A Jira administrator to install and set up the app.

## Install and connect

1. A Jira administrator installs the app from the Atlassian Marketplace.
2. Open **Settings > Apps > Ops Essentials**.
3. In **Overview**, press **Test connection**. When Atlassian asks for it, choose **Allow access**.

Atlassian's operations API does not accept calls made by an app on its own, so the background checks act on
behalf of the administrator who set up the connection. **That person must keep access to Jira Service
Management Operations.** If they leave or lose access, another administrator opens **Overview** and presses
**Test connection** again: from then on the background checks act on their behalf.

**Overview** shows the state of the connection, of the background job (it runs every 5 minutes), of the
heartbeats and of the alert archive. Heartbeats and background jobs are paused while the app's license is not
active.

## Heartbeats

A heartbeat watches a job that runs on a schedule: a backup, a cron job, an integration. The job calls the
heartbeat's address at the end of every successful run. If the call does not arrive in time, the app opens an
alert in Jira Service Management.

### Create a heartbeat

In **Heartbeats**, create a new heartbeat and choose:
- a name;
- how often the job is expected to check in, from **5 minutes to 1 week**;
- an optional extra grace period, **up to 1 hour**;
- the priority of the alert, **P1 to P5**;
- the **team** responsible: the alert follows that team's on-call schedules and escalations.

Then press **URL** on the heartbeat to see its address.

### Call the address

Call the address at the end of every successful run. Any HTTP method works. Optional text in the request body
is shown in the alert.

```bash
curl -fsS -m 10 --retry 3 https://<your-heartbeat-address>
```

To report failures too, call the failure address (the heartbeat address followed by `/fail`) when the job
fails. For example, a backup that runs every night at 2:00:

```bash
0 2 * * * /opt/backup.sh && curl -fsS -m 10 https://<your-heartbeat-address> || curl -fsS -m 10 https://<your-heartbeat-address>/fail
```

### What happens

- **No call in time** (interval plus grace period): an alert opens for the team you chose. Heartbeats are
  checked every 5 minutes, so the alert can open up to 5 minutes after the deadline.
- **The next successful call closes the alert.**
- **Failure address** (`/fail`): the alert opens immediately, with the text sent by the job (for example its
  exit code).
- **Pause** stops the checks of a heartbeat; **Resume** starts them again.
- **Deleting a heartbeat** makes its address stop working and closes its open alert, if any.
- Each address contains a random token of 32 characters. If an address leaks, regenerate its token: the
  address changes, so update the jobs that call it.

## Reports

In **Reports**, choose a period (for example the last 7, 30 or 90 days) and, if you want, a team. The report
shows:
- the number of alerts, and of occurrences including duplicates;
- **time to acknowledge** and **time to resolve**: median, average and 90th percentile;
- the share of alerts acknowledged within **5 and 15 minutes**;
- the share of alerts **outside office hours** (weekends and 18:00-08:00 UTC);
- the **noisiest alerts**;
- **who handled alerts**: workload per responder and per team;
- charts by day and by hour of the day.

Reports are built from the alert archive (see below).

## Stakeholder updates

Send short updates about alerts to people who do not work in Jira, such as managers or customer support.
Stakeholders need no Jira license.

### Channels

In **Stakeholders**, add one or more channels:
- **Slack:** an incoming webhook.
- **Microsoft Teams:** a webhook created with Workflows.
- **Google Chat:** a webhook of the space.
- **E-mail:** through **SendGrid** or **Resend**, with your API key, the recipients (separated by commas) and a
  sender address verified with your e-mail provider.

Press **Send test** to check each channel. Webhook addresses and API keys are stored encrypted and are never
shown again after you save them.

### Rules

A rule decides which alerts are announced, when and where:
- **events:** alert opened, acknowledged, closed;
- **filters:** priorities, teams and tags (leave them empty for all alerts);
- **channels:** one or more;
- **messages** (optional): your own text for each event, with these placeholders: `{{message}}`
  `{{priority}}` `{{tinyId}}` `{{owner}}` `{{duration}}` `{{link}}` `{{createdAt}}`.

A rule can be paused and resumed.

### Timing and delivery log

- An update can arrive **up to 5 minutes** after the alert changes. If an alert opens and closes between two
  checks, both messages are still sent.
- **Recent updates** lists the updates sent, with the result of each one.

## Alert archive

- On the first run the app imports past alerts for the period chosen in **Settings** (at most 20,000 alerts, a
  limit of Atlassian's API). After that, every new alert is copied every 5 minutes.
- In **Archive**, search by text, alias or number (for example `#123`) and filter by period, team and
  priority.
- **Export CSV** creates a one-time download link, valid for 10 minutes.
- In **Settings** you choose how many months to keep archived alerts (0 keeps them forever). Older alerts are
  deleted once a day.

## Settings

- **Keep the alert archive and reports up to date:** needed for reports, the archive and stakeholder updates.
  Heartbeats work either way.
- **Import history on first run:** how far back to import past alerts.
- **Keep archived alerts for (months):** 0 keeps them forever.
- **Import history again:** imports the chosen period again. Alerts already in the archive are updated, not
  duplicated.

## Troubleshooting

- **Test connection fails:** check that Operations is turned on for at least one team and that you have access
  to Jira Service Management Operations. The message on the Overview tab shows what failed.
- **A heartbeat alert does not open or close:** check the background job on the **Overview** tab and the time
  of the last call on the **Heartbeats** tab. Run the `curl` command by hand on the job's server: with
  `-fsS` it prints any error.
- **A stakeholder update does not arrive:** check its result in **Recent updates** and press **Send test** on
  the channel.

If the problem remains, see [Support](https://steadyseam.com/support.html#ops-essentials-for-jsm).

## Data, privacy and security

- All data stays in your site's Forge storage (Forge key-value store and Forge SQL), hosted by Atlassian.
  Steadyseam has no servers of its own and does not receive or see your data.
- Data leaves Atlassian only if you configure a stakeholder channel: then the alert number, text, priority,
  owner, duration and link are sent to the webhook or e-mail service you chose, with your credentials.
- No usage analytics and no tracking.

Details: [privacy policy](privacy-policy.md), [Data Processing Addendum](dpa.md), [security](security.md).

## Uninstalling

Remove the heartbeat calls from your jobs. When the app is uninstalled, Atlassian deletes the data stored by
the app according to its data retention policy for Forge apps.
