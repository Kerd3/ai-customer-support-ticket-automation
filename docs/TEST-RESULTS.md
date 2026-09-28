# Test Results

## High-Priority Approval Test

Ticket #10 completed the high-priority path.

Observed classification and routing:

- Category: billing
- Priority: high
- Assigned team: Billing Team
- Slack escalation: recorded
- Human approval: approved
- Customer reply: sent

Audit sequence:

```text
ticket_created
ticket_assigned
high_priority_alert_sent
response_drafted
approval_requested
response_approved
customer_reply_sent
```

Evidence: [High-priority Slack alert](../screenshots/05-slack-high-priority-alert.png)

## Rejection Test

Ticket #11 completed the rejection path and was reopened.

Observed final audit event:

```text
ticket_reopened
```

The ticket response was rejected and the ticket status returned to `open`.

Evidence: [Approval / rejection flow](../screenshots/07-approval-rejection-flow.png)

## Low-Priority Test

Ticket #13 completed the normal low-priority path.

Observed state:

- Category: account
- Priority: low
- Assigned team: Account Support
- Status: sent
- Approved: true

Audit sequence:

```text
ticket_created
ticket_assigned
response_drafted
approval_requested
response_approved
customer_reply_sent
```

No `high_priority_alert_sent` event was recorded.

Evidence: [PostgreSQL audit trail](../screenshots/08-postgresql-audit-trail.png)

## Final Clean End-to-End Test

Ticket #21 completed a fresh low-priority approval flow after the intentional failure test had been restored.

Observed state:

- Category: account
- Priority: low
- Assigned team: Account Support
- Status: sent
- Approved: true
- Customer reply sent: yes

Audit sequence:

```text
ticket_created
ticket_assigned
response_drafted
approval_requested
response_approved
customer_reply_sent
```

Evidence: [PostgreSQL audit trail](../screenshots/08-postgresql-audit-trail.png)

## Duplicate Detection Test

A repeated Gmail submission with the same sender, subject, and message body returned:

```json
{
  "duplicate": true
}
```

The duplicate path therefore stopped the ticket from entering AI classification and ticket creation.

Evidence: [Duplicate detection result](../screenshots/10-duplicate-detection-result.png)

## Error Handling Test

A controlled PostgreSQL failure was introduced in `Log Ticket Created` using an invalid SQL statement.

The real execution produced:

```text
Workflow: AI Customer Support Ticket Automation
Failed Node: Log Ticket Created
Execution ID: 593
```

The failure was:

```text
Syntax error at line 1 near "INVALID"
```

The error handler successfully:

1. Received the failed execution through Error Trigger.
2. Logged the failure to `workflow_errors`.
3. Sent the failure details to Slack.

The invalid SQL was then restored before the final clean test.

Evidence: [Error handler workflow](../screenshots/11-error-handler-workflow.png) and [real Slack error notification](../screenshots/12-error-slack-notification.png)
