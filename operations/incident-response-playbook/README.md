# Incident Response Playbook

> Step-by-step guide for handling production incidents and outages

**Domain:** operations · **Version:** 1.0.0 · **By:** Yong (Kavela)

**Keywords:** `incident` `outage` `production` `response` `escalation` `postmortem` `on-call` `alert`

[View on Kavela Marketplace](https://kavela.ai/marketplace/incident-response-playbook)

---
# Incident Response Playbook

## Severity Levels
| Level | Definition | Response Time | Example |
|-------|-----------|---------------|---------|
| SEV1 | Complete outage, all users affected | 15 min | Site down, data loss |
| SEV2 | Major feature broken, many users affected | 30 min | Payments failing |
| SEV3 | Minor feature broken, some users affected | 2 hours | Search slow |
| SEV4 | Cosmetic/minor issue | Next business day | Typo in UI |

## Response Steps

### 1. Acknowledge (within response time)
- Join #incidents Slack channel
- Claim the incident: "I am IC for this incident"
- Create incident ticket in Linear

### 2. Assess
- What is the impact? (users affected, revenue impact)
- When did it start? (check monitoring dashboards)
- What changed recently? (deploys, config changes)

### 3. Mitigate
- Can we rollback the last deploy?
- Can we toggle a feature flag?
- Can we scale up resources?
- Priority: **stop the bleeding first**, investigate root cause later

### 4. Communicate
- Update status page within 30 min of SEV1/SEV2
- Post updates every 30 min until resolved
- Template: "We are aware of [issue]. [X] users are affected. We are [action]. Next update in [time]."

### 5. Resolve & Document
- Confirm service is restored
- Update status page to resolved
- Schedule postmortem within 48 hours

## Postmortem Template
- Timeline of events
- Root cause analysis (5 Whys)
- What went well
- What could be improved
- Action items with owners and due dates

## Escalation Path
On-call engineer → Team lead → Engineering manager → CTO
