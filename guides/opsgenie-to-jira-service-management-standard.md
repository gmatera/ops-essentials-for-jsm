# Moving from Opsgenie to Jira Service Management Standard: what to plan for

Atlassian stopped selling Opsgenie on **4 June 2025**, and Opsgenie support ends on **5 April 2027**. On that
date access shuts off, and any data that has not been migrated is deleted
([Atlassian](https://www.atlassian.com/software/opsgenie/migration)). The recommended destination for alerting
and on-call is Jira Service Management, and Atlassian provides a migration path for teams, schedules,
escalations, integrations and alerts.

The migration itself is well documented. What is easy to miss is how the **plan** you choose in Jira Service
Management changes your teams' daily routines. This guide lists the points worth checking before you move,
especially if you are going to the Standard plan.

## 1. Inventory what your teams actually use

Before comparing plans, list what Opsgenie does for you today, not what it could do:

- alert sources (monitoring tools, e-mail, API integrations) and their volume;
- on-call schedules, rotations and escalation policies;
- **heartbeats** for backups, cron jobs and integrations;
- reports that someone reads every week or month (time to acknowledge, time to resolve, after-hours load);
- who outside the on-call teams is informed about incidents, and how (Slack, Teams, e-mail);
- how long you need to keep alert history, and who searches it.

The last three items are the ones that most often get lost, because nobody thinks of them as features.

## 2. Check each item against the plan

Alerting, on-call schedules and escalations are part of Jira Service Management's operations features.
Some capabilities depend on the plan. One example that matters to many teams: **heartbeat monitoring is not
available on the Free and Standard plans** ([Atlassian](https://support.atlassian.com/opsgenie/docs/advanced-alert-configurations-after-migration/)).
Opsgenie heartbeats on those sites need either an upgrade or a replacement.

For reporting, stakeholder communication and data retention, open the current plan comparison for your site
and check each item from your inventory. Plans change over time; the comparison page is the only reliable
source.

## 3. Decide per item: upgrade, replace or drop

For each gap there are three honest answers:

| Answer | When it makes sense |
|---|---|
| Upgrade the plan | Several gaps at once, or you need other Premium features anyway |
| Replace the single feature | One or two gaps, many agents, and the rest of Standard fits |
| Drop it | Nobody would notice: the report nobody reads, the heartbeat for a job that no longer exists |

Be strict with the third row. A migration is a good moment to delete heartbeats for jobs that were
decommissioned years ago.

## 4. Plan the heartbeats carefully

Heartbeats are the item with the highest risk: if they disappear, nothing tells you. A backup that stopped
running is discovered on the day you need the backup.

- Export the list of Opsgenie heartbeats with their intervals and owning teams.
- Mark the ones that still matter. For each one, note which job calls it and from which server.
- Move them before you turn Opsgenie off, and run old and new side by side for a few days.
- Test every heartbeat once on purpose: stop the job, or call the failure URL, and check that the right team
  is alerted.

See [Heartbeat monitoring for cron jobs and backups](heartbeat-monitoring-cron-jobs-jira-service-management.md)
for practical examples on Linux and Windows.

## 5. Keep people outside the on-call teams informed

In many companies managers and customer support do not have a Jira license, but they need to know when a major
incident starts and when it ends. If today they receive Opsgenie notifications or stakeholder messages, decide
before the switch how they will be informed afterwards. See
[Incident updates for people without a Jira license](incident-updates-for-stakeholders-without-jira-license.md).

## 6. Keep the history you need

If audits, customer reviews or post-incident reviews need alert history older than your plan keeps, export or
archive it before it ages out, and decide where it will live.

## Checklist

- [ ] Inventory of features in real use, including heartbeats, reports, stakeholder updates and history
- [ ] Each item checked against the plan comparison of your site
- [ ] Decision per item: upgrade, replace or drop
- [ ] Heartbeats moved and tested one by one, before Opsgenie is turned off
- [ ] Stakeholders' new channel agreed and tested
- [ ] Alert history archived or exported where needed
- [ ] Opsgenie turned off well before 5 April 2027

---

*This guide is written by the developer of **Ops Essentials for Jira Service Management**, an app that adds
heartbeats, operations reports, stakeholder updates and a long-term alert archive to Jira Service Management,
including the Standard plan. 30-day free trial.
[Learn more](https://marketplace.atlassian.com/apps/2213311074/ops-essentials-for-jira-service-management?utm_source=site&utm_medium=guide&utm_campaign=opsgenie-migration) · [User guide](../user-guide.md)*
