# Incident updates for people without a Jira license: Slack, Teams, Google Chat and e-mail

When a major incident starts, the on-call team is busy fixing it. At the same time managers, customer support
and account managers need to know what is happening: is it affecting customers, is someone on it, is it over?
Most of them do not have a Jira license, and should not need one to receive a two-line update.

Without a plan, the same thing happens every time: the on-call engineer is interrupted by messages asking for
status, and somebody pastes screenshots into a chat. This guide describes a simple setup that keeps
stakeholders informed automatically and keeps the on-call team focused.

## Decide who needs what

Start from the audience, not from the tool:

| Audience | What they need | Usual channel |
|---|---|---|
| Management | Major incidents only: started, resolved, how long | E-mail or a management channel |
| Customer support | Anything customers may notice, as early as possible | Their team chat |
| Account managers | Incidents affecting specific customers or regions | Chat or e-mail, filtered by tag |
| Other engineering teams | Incidents on services they depend on | Their team chat |

## Keep the rules few and strict

- **Filter by priority.** Most stakeholders only need P1 and P2. Every extra message makes the important ones
  easier to miss.
- **Send opened and closed, not every change.** "Acknowledged" is useful for support ("someone is on it"),
  rarely for management.
- **Use tags for routing**, for example `customer-facing` or a region, when one audience only cares about part of
  the alerts.
- **One channel per audience.** Do not send the same update to five channels "just in case".

## Write messages that need no context

A good update can be read on a phone, without opening anything:

```
[P1] New alert #482: Checkout API error rate above 5% (EU)
[P1] Resolved: alert #482 (Checkout API error rate above 5% (EU)) after 37m
```

Priority, number, short description, and for the closing message how long it lasted. Add a link for those who
have access; the others still understand the message.

## Setting up the channels

- **Slack:** create an incoming webhook for the channel (Slack app settings › Incoming Webhooks). The webhook URL
  is a secret: whoever has it can post in the channel.
- **Microsoft Teams:** create a webhook for the channel with the Workflows app (Power Automate), or with an
  incoming webhook where your tenant still allows them.
- **Google Chat:** in the space, open Apps & integrations › Webhooks and add one.
- **E-mail:** send through a provider such as SendGrid or Resend, from a sender domain you have verified, so the
  messages are not marked as spam. Use a distribution list instead of individual addresses, so people can be
  added and removed without touching the configuration.

Send a test message to every channel before relying on it, and send it again after any change on the chat side:
webhooks are often deleted when a channel is archived or renamed.

## Keep it private and safe

- Store webhook URLs and API keys like passwords, and never paste them in tickets or documentation.
- Updates should contain only what the audience needs. Alert descriptions sometimes include host names or
  customer data: keep those for the on-call team.

## Checklist

- [ ] Audiences and what each one needs agreed
- [ ] One channel per audience, with a test message received
- [ ] Rules filtered by priority (and tags where needed)
- [ ] Message format readable without opening Jira
- [ ] Webhook URLs and API keys stored as secrets

---

*This guide is written by the developer of **Ops Essentials for Jira Service Management**, which sends
stakeholder updates from Jira Service Management alerts to Slack, Microsoft Teams, Google Chat or e-mail, with
rules by priority, team and tag, and adds heartbeats, operations reports and a long-term alert archive.
30-day free trial.
[Learn more](https://marketplace.atlassian.com/apps/2213311074/ops-essentials-for-jira-service-management?utm_source=site&utm_medium=guide&utm_campaign=stakeholder-updates) · [User guide](../user-guide.md)*
