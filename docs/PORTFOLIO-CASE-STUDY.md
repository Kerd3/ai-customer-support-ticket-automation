# Portfolio Case Study

## AI-Powered Support Ticket Triage & Response Automation

### Problem

Customer support requests arriving through email can require repetitive triage, manual routing, response preparation, and follow-up. The goal of this project was to automate the repetitive workflow while keeping human control over customer-facing communication.

### Solution

I built an n8n-based workflow that receives Gmail support emails, normalizes their contents, checks for duplicates, uses an LLM to classify the request, validates the structured result, creates a PostgreSQL ticket, automatically assigns a support team, escalates high/critical issues to Slack, drafts a response, and pauses for human approval before sending the reply.

A separate audit trail records the lifecycle of each ticket, while an Error Trigger workflow logs failures and sends operational alerts.

### Key Engineering Decisions

**Human approval before external communication**

The AI can draft a reply but cannot independently send it. Approval or rejection determines whether the reply is sent or the ticket is reopened.

**Structured AI output validation**

The LLM is constrained to explicit category, priority, sentiment, and boolean values. A JavaScript validation step rejects missing or invalid output before database insertion.

**Duplicate prevention**

The workflow first checks the unique Gmail message ID and also detects repeated customer/subject/message combinations within a recent time window.

**Auditability**

Ticket lifecycle events are stored separately from the current ticket state so the project can show how a ticket moved through the workflow.

**Failure handling**

Workflow failures are captured by a separate Error Trigger workflow, stored in PostgreSQL, and sent to Slack for visibility.

### Verification

The implementation was verified through actual n8n executions covering:

- high-priority routing and Slack escalation
- low-priority routing
- approval and customer reply
- rejection and ticket reopening
- duplicate detection
- controlled PostgreSQL failure handling
- error logging and Slack notification

### Result

The completed workflow demonstrates an AI-assisted automation pattern where deterministic workflow logic controls persistence, validation, routing, approvals, communication, and failure handling around the LLM.
