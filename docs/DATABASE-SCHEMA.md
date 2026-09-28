# Database Schema

## tickets

Stores the current support ticket record.

| Column | Purpose |
|---|---|
| id | Internal ticket ID |
| external_message_id | Gmail message ID used for duplicate detection |
| customer_email | Customer email address |
| customer_name | Customer name |
| subject | Email subject |
| message | Full customer message |
| category | AI classification category |
| priority | AI priority classification |
| sentiment | AI sentiment classification |
| summary | Concise issue summary |
| suggested_action | Recommended support action |
| status | Current ticket state |
| assigned_team | Automatically assigned support team |
| requires_human | Human-review requirement |
| created_at | Ticket creation timestamp |
| updated_at | Last ticket update timestamp |

## ticket_responses

Stores generated customer responses and approval state.

| Column | Purpose |
|---|---|
| id | Response ID |
| ticket_id | Related ticket |
| draft_response | AI-generated draft |
| final_response | Stored final response |
| approved | Human approval state |
| approved_at | Approval timestamp |
| sent_at | Customer send timestamp |
| created_at | Draft creation timestamp |

## ticket_events

Stores chronological ticket lifecycle events.

| Column | Purpose |
|---|---|
| id | Event ID |
| ticket_id | Related ticket |
| event_type | Lifecycle event name |
| event_data | Structured JSON event details |
| created_at | Event timestamp |

## workflow_errors

Stores execution failures from the separate Error Trigger workflow.

| Column | Purpose |
|---|---|
| id | Error record ID |
| workflow_name | Name of failed workflow |
| workflow_id | n8n workflow ID |
| execution_id | Failed execution ID |
| error_node | Last/failed node reported by n8n |
| error_message | Failure message |
| execution_url | n8n execution reference |
| created_at | Error timestamp |
