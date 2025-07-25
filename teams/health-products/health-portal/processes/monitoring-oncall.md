# Monitoring and On-Call Rotation

Created: 2025-07-25

## Overview

The Health Portal on VA.gov relies on multiple systems to enable the health experience. When something goes wrong with VA.gov or any of the systems we rely on, we should know what to do or who to reach out to. The on-call rotation places responsibility on a few team members at any time for responding to those situations.

## How we work

This process will evolve as VA tools and capabilities allow us to mature our capabilities.[^1] For the immediate future, we'll need to actively monitor Slack channels for errors from our [MHV Datadog monitors](https://vagov.ddog-gov.com/monitors/manage?q=team%3Amhv).[^2] In the near future, we should be able to have PagerDuty alert engineers when something goes wrong.

### Late 2025 process

An engineer from each tool team will be on-call for a 7 day period, with a new rotation starting every Thursday. On-call engineers will monitor their tools' Slack `-alerts` channel and the `mhv-on-vagov-alerts` channel. The on-call engineer will investigate issues that arise, and coordinate with relevant partners, e.g. MHV Backend, on actions needed to resolve issues.

The OCTO Watch Officer on-call may reach out when a Priority 1 and 2 (P1 and P2) monitor reaches or maintains an "error" state. The on-call engineer will take responsibility to respond to the Watch Officer and provide updates and resolution when needed.

### Future process

When PagerDuty is migrated and configured to alert engineers when something goes wrong, periodic checking of Slack will be less important. Instead, engineers will acknowledge notifications from PagerDuty and begin the triage process in response.

## How to triage

When we see an alert triggered by Datadog, we need to try to understand the severity, systems involved, and possible path to recovery. We should ask ourselves:

- Is the issue with a single endpoint or tool? Or does it affect multiple tools, or all of VA.gov?
- Does the tool/service appear to be completely down, or experiencing degraded performance?
- Does the issue appear to be a result of a problem with an upstream system, like MHV API? MHV API might retransmit errors from the systems _it_ relies on, such as VistA or Oracle Health.
- Does the issue appear to be getting worse, or recovering? This may be an indicator of a system operating at or near capacity.

VA.gov and the MHV Frontend (`vets-website` and `vets-api`) depend on many other systems for data and functionality, and we should be prepared to inform and engage with those system owners to resolve issues

### Caveats

The alerts that come from Datadog can tell you when something is happening, but won't provide clear information about what is going wrong. The VA's monitors often use a measurement tool called [`statsd`](https://github.com/statsd/statsd) to monitor for statistical anomalies, but at the cost of links to logs that can provide more detail.


## Relevant Links

- [Platform OnCall docs](https://github.com/department-of-veterans-affairs/va.gov-team-sensitive/tree/master/OnCall)
- [Datadog](https://vagov.ddog-gov.com/): the tool that provides monitoring and logging capabilities
- [PagerDuty](https://dsva.pagerduty.com/): provides notifications, escalation, and on-call scheduling capabilities.[^1]
- Slack channels
  - mhv-on-vagov-alerts
  - mhv-medical-records-alerts
  - mhv-medications-alerts-issues
  - mhv-secure-messaging-alerts
  - va-cto-supply-reordering-alerts
  - mhv-platform-alerts





## Notes

[^1]: As of July 2025, PagerDuty has not been configured for notifications, escalation or on-call rotation scheduling. Current use is for controlling maintenance windows. Full use of PagerDuty functionality is pending a migration to a new PagerDuty instance.

[^2]: There is ongoing work to improve the Datadog monitors to improve signal-to-noise and trigger "error" states when something is truly wrong. As of July 2025, monitors need work to provide high signal and low noise.
