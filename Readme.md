# AI-Powered Bug Triage System with OpenAI, Jira and Slack Alerts

This workflow ingests bug reports via a webhook, uses OpenAI to triage and tag them, creates Jira bugs with AI-generated labels, and sends priority-based alerts to Slack channels — making bug handling faster, more consistent, and instantly visible to your QA or engineering teams.

---

## Quick Start – Implement in 60 Seconds

1. Import the workflow JSON into your n8n instance.
2. Add credentials for **OpenAI**, **Jira**, and **Slack**.
3. Set the webhook path (`bug-suspicion`) in the Webhook Trigger node.
4. Configure your Slack channels (e.g., `qa-alerts-high`, `qa-general`).
5. Activate the workflow and test with a POST payload containing `title`, `summary`, etc.

---

## What It Does

- Receives bug report payloads via webhook.
- Sends the report to **OpenAI** for priority and category analysis.
- Routes based on AI priority to create a **Jira Bug** with standardized labels.
- Sends Slack alerts:
  - High-priority bugs → `qa-alerts-high` channel
  - Normal/low-priority → `qa-general` channel

---

## Who’s It For

- QA teams wanting automated Jira escalation.
- Developers integrating external systems with Jira.
- Product teams capturing automated “bug suspicion” signals.
- Monitoring or observability pipelines (Sentry, internal tools).
- Teams wanting instant visibility for prioritized issues.

---

## Requirements

- n8n (cloud or self-hosted) instance.
- **OpenAI API key** for AI analysis.
- **Jira credentials** with permission to create issues in your project.
- **Slack bot token** with `chat:write` permissions.
- Ability to send POST requests to n8n’s webhook endpoint.

---

## How It Works & Setup Instructions

### 1. Webhook Trigger

Configure the **Webhook Trigger** node:

- Method: `POST`
- Path: `bug-suspicion`
- Example POST payload:

```json
{
  "title": "Login button unresponsive",
  "summary": "User cannot click login button",
  "category": "UI"
}
```

This starts the workflow when a bug report is sent to the webhook.

### 2. AI Bug Analysis (OpenAI)

The **OpenAI** node analyzes the report fields to determine:

- Priority (e.g., High, Medium, Low)
- Suggested category
- Context for Jira description

### 3. Priority Routing

A **Switch** node checks the AI-determined priority and directs each report down the appropriate branch (e.g., high or normal).

### 4. Create Jira Bug

For each priority branch, a **Jira** node creates a bug ticket in your configured project with labels like:

```json
{
  "labels": ["ai-triaged", "bug-suspicion"]
}
```

The description can include structured data such as severity, category, and original summary.

### 5. Slack Alerts

Slack nodes notify your team:

- **High priority →** `qa-alerts-high`
- **Normal priority →** `qa-general`

Alerts include the Jira issue key and summary.

---

## How to Customize Nodes

### Webhook Trigger

- Add API keys, tokens or Basic Auth to secure webhook traffic.
- Add JSON schema validation to ensure required fields.

### AI Bug Analysis

- Customize the prompt to refine priority and category logic.
- Add additional fields (e.g., error logs, reproduction steps).

### Jira Node

Add or customize fields:

```json
"additionalFields": {
  "labels": "bug-suspicion,auto-detected",
  "description": "={{$json[\"details\"]}}"
}
```

### Slack Node

- Adjust message formatting.
- Add mentions, attachments, or threads for richer notifications.

---

## Add-Ons (Optional Enhancements)

- Email alerts for high priority bugs.
- Severity scoring using AI confidence levels.
- Push data to Notion or Google Sheets for audit logs.
- Add screenshots or logs from the original report.
- Multi-channel notifications (Teams, SMS).
- Auto-assign Jira issues based on category or team workload.

---

## Use Case Examples

1. Automated QA test failure reporting → Jira + Slack alerts.
2. Monitoring tools sending bug data (Sentry, Prometheus, etc.).
3. Customer feedback forms triggering bug tickets automatically.
4. CI/CD pipeline errors escalating to developers.
5. Integration with customer support systems.

---

## Common Troubleshooting Guide

| Issue                      | Possible Cause                  | Solution                    |
| -------------------------- | ------------------------------- | --------------------------- |
| Webhook not receiving data | Wrong URL or method             | Ensure POST & correct path  |
| Jira ticket not created    | Invalid credentials/project     | Verify Jira API settings    |
| Slack alert not sent       | Bot not allowed in channel      | Invite bot and check scopes |
| Jira fields missing        | Payload missing required fields | Validate input JSON         |
| Slack alert undefined      | Jira response output changed    | Use Debug node to inspect   |
| Workflow not running       | Workflow inactive               | Turn on Active toggle       |

---

## Need Help?

If you’d like assistance customizing or extending this workflow — such as adding CRM integrations, advanced AI scoring, or multi-tool routing — the **WeblineIndia** n8n automation team can help you with:

- Jira workflow automation
- Slack alerts & prioritization
- API integrations and security
- AI prompt enhancements
- Scalable automation solutions for your team

Reach out anytime for
