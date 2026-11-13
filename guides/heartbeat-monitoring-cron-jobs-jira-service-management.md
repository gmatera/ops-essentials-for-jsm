# Heartbeat monitoring for cron jobs and backups in Jira Service Management

Most monitoring watches things that are running: servers, services, response times. A scheduled job that
**stops running** produces no error at all. The backup script that was removed during a server migration, the
cron entry commented out "just for today", the integration whose credentials expired: they fail silently,
until someone needs their output.

A heartbeat turns that silence into an alert. The job calls a URL every time it completes. If the call does not
arrive in time, an alert opens for the team that owns the job. This guide shows how to set heartbeats up so that
they catch real problems without waking people up for nothing.

## How a heartbeat works

1. You create a heartbeat with an **expected interval** (for example 1 day for a nightly backup) and a **grace
   period** for normal variations in run time.
2. The job calls the heartbeat URL at the end of every **successful** run.
3. If no call arrives within interval plus grace, an alert opens in Jira Service Management for the chosen team,
   and follows its on-call schedule and escalation.
4. The next successful call closes the alert.

Many heartbeat tools also offer a **failure URL**: the job calls it when it fails, and the alert opens at once,
without waiting for the deadline, ideally with the job's error output.

Note that Jira Service Management's own heartbeats are not available on the Free and Standard plans
([Atlassian](https://support.atlassian.com/opsgenie/docs/advanced-alert-configurations-after-migration/)).
The patterns below work with any heartbeat URL.

## Linux: cron

Call the URL only if the job succeeded, and the failure URL otherwise:

```sh
0 2 * * * /opt/backup.sh && curl -fsS -m 10 --retry 3 https://example.com/hb/TOKEN || curl -fsS -m 10 --retry 3 --data "exit code $?" https://example.com/hb/TOKEN/fail
```

- `-m 10` limits each attempt to 10 seconds, so a network problem never blocks the job.
- `--retry 3` handles short network glitches.
- `-fsS` keeps cron mail quiet on success and shows errors when the call itself fails.

For longer scripts, send the end of the log with the failure, so the responder sees it in the alert:

```sh
if /opt/backup.sh > /var/log/backup.log 2>&1; then
  curl -fsS -m 10 --retry 3 https://example.com/hb/TOKEN
else
  tail -n 20 /var/log/backup.log | curl -fsS -m 10 --retry 3 --data-binary @- https://example.com/hb/TOKEN/fail
fi
```

## Linux: systemd timers

Add the call to the service the timer starts. `ExecStartPost` runs only when the main command succeeds:

```ini
[Service]
Type=oneshot
ExecStart=/opt/backup.sh
ExecStartPost=/usr/bin/curl -fsS -m 10 --retry 3 https://example.com/hb/TOKEN
```

## Windows: Task Scheduler and PowerShell

```powershell
& C:\scripts\backup.ps1
if ($LASTEXITCODE -eq 0) {
  Invoke-RestMethod -Method Post -Uri 'https://example.com/hb/TOKEN' -TimeoutSec 10
} else {
  Invoke-RestMethod -Method Post -Uri 'https://example.com/hb/TOKEN/fail' -Body "exit code $LASTEXITCODE" -TimeoutSec 10
}
```

## Choosing interval and grace period

- **Interval** = how often the job is scheduled. A nightly job has 1 day, a job every 15 minutes has 15 minutes.
- **Grace** = the longest normal delay. Look at the slowest runs of the last months, not the average. A backup
  that usually takes 20 minutes but sometimes 2 hours needs at least 2 hours of grace.
- Too short a grace period produces alerts that people learn to ignore. That is worse than no heartbeat.

## Avoiding false alarms

- **Pause** a heartbeat during planned maintenance, and resume it afterwards.
- One heartbeat per job, not per server: if a job moves to another server, the heartbeat follows the job.
- Give each heartbeat a name that a responder understands at 3 a.m., and a short note on what to check first.
- Test each heartbeat once on purpose: call the failure URL and check that the right team is alerted.

## Keep the URL private

Treat the heartbeat URL as a credential: anyone who has it can report a success or a failure. Keep it in the
job's configuration, not in public repositories, and replace it if it leaks.

---

*This guide is written by the developer of **Ops Essentials for Jira Service Management**, which adds
heartbeats with failure URLs, operations reports, stakeholder updates and a long-term alert archive to Jira
Service Management, including the Standard plan. 30-day free trial.
[Learn more](https://marketplace.atlassian.com/apps/2213311074/ops-essentials-for-jira-service-management?utm_source=site&utm_medium=guide&utm_campaign=heartbeats) · [User guide](../user-guide.md)*
