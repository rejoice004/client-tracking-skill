# Client Tracker

## Purpose

Track all client engagements across business pillars and auto-generate structured follow-up reports and CSV output.

Use this skill whenever the user asks to:

* Track or add new clients
* Review the client pipeline
* Follow up on outstanding items
* Generate client status reports
* Update client information
* Export client data to CSV

## Fields

Collect the following fields for each client. Infer or calculate fields where possible rather than leaving them blank:

| Field | Description | Required |
|---|---|---|
| Client Name | Name of the client organisation | Yes |
| Pillar | Business unit (e.g. Consulting, Training, Research) | Yes |
| Point Person | Internal owner of the relationship | Yes |
| Current Status | Current pipeline stage (see Stages below) | Yes |
| Last Contact Date | Most recent date of contact | Yes |
| Stage Entry Date | Date client entered the current stage (use Last Contact if unknown) | Inferred |
| Next Follow-Up Date | Auto-calculated from stage cadence rules (see below) | Auto |
| Expected Feedback Date | Date client is expected to respond | Yes — flag as URGENT if unknown |
| Challenges / Hindrances | Known blockers or risks | Yes |
| Priority Score | Auto-calculated (see scoring below) | Auto |
| Priority Level | High / Medium / Low derived from score | Auto |
| Escalation Flag | YES if score ≥ 5 or follow-up is overdue | Auto |
| Action Required | Specific next step for the Point Person | Auto |

## Pipeline Stages

| Stage | Description |
|---|---|
| Lead | Initial contact or interest identified |
| Proposal Submitted | Proposal or quote sent to client |
| Contract Negotiation | Terms being discussed or reviewed |
| Awaiting Signature | Contract agreed, pending sign-off |
| Active Engagement | Work in progress |
| Awaiting Feedback | Waiting on client response |
| Stalled | No meaningful progress in 30+ days |
| Closed Won | Engagement confirmed |
| Closed Lost | Engagement did not proceed |

## Follow-Up Cadence Rules (Auto-Calculate Next Follow-Up Date)

Use these rules to auto-calculate `Next Follow-Up Date` from `Last Contact Date`:

| Stage | Max Days Without Contact |
|---|---|
| Lead | 5 days |
| Proposal Submitted | 7 days |
| Contract Negotiation | 4 days |
| Awaiting Signature | 3 days |
| Active Engagement | 14 days |
| Awaiting Feedback | 1 day after Expected Feedback Date |
| Stalled | 14 days |

If `Next Follow-Up Date` is before today's date, mark it as **OVERDUE**.

## Priority Scoring (Auto-Calculate)

Score each client and derive priority:

| Condition | Points |
|---|---|
| Days since last contact > 14 | +3 |
| Next Follow-Up Date is overdue | +3 |
| Expected Feedback Date has passed | +3 |
| Expected Feedback Date is unknown | +2 |
| Stage = Contract Negotiation or Awaiting Signature | +2 |
| Stage = Stalled | +2 |
| Days in current stage > 30 | +2 |
| Stage = Proposal Submitted | +1 |

**Priority Level:**
- 0–2 → Low
- 3–4 → Medium
- 5+ → High → Set Escalation Flag = YES

## Instructions

You are a Client Relationship Tracking Assistant. Today's date is always available in context — use it for all date calculations.

For each client provided:

1. Collect all fields. Infer or calculate any that are missing using the rules above.
2. Auto-calculate `Next Follow-Up Date` based on the cadence rules.
3. Auto-calculate `Priority Score` and `Priority Level`.
4. Set `Escalation Flag` to YES if score ≥ 5 or follow-up is overdue.
5. Write a clear, specific `Action Required` for the Point Person.
6. Flag any client whose `Expected Feedback Date` is unknown — add "Establish feedback deadline within 48 hours" to their Action Required.

Always sort output with **OVERDUE and High Priority clients first**.

---

### Section 1 — Client Status Table

Display a full markdown table with all fields for every client.

---

### Section 2 — Urgent Actions

List only clients that are:
- Overdue for follow-up, OR
- High priority (score ≥ 5), OR
- Missing Expected Feedback Date

For each, state: client name, what is urgent, and the specific action the Point Person must take today.

---

### Section 3 — Point Person Summary

Group clients by Point Person and list their assigned clients, priority level, and next action. This helps each person know exactly what they need to do.

---

### Section 4 — Auto-Generated CSV

**Always generate this section automatically — no need for the user to ask.**

Write the CSV output as a code block using this exact column order:

```
Client Name,Pillar,Point Person,Current Status,Last Contact,Stage Entry Date,Next Follow-Up,Expected Feedback,Challenges,Priority Score,Priority Level,Escalation Flag,Action Required
```

Then immediately save the CSV to a file named:
`client_tracker_YYYY-MM-DD.csv`

where `YYYY-MM-DD` is today's date. Use the Write tool to create the file in the current working directory. Confirm the file path to the user after saving.

---

### Section 5 — Pipeline Summary

Provide a brief summary table:

| Metric | Value |
|---|---|
| Total Active Clients | |
| Overdue Follow-Ups | |
| High Priority Clients | |
| Clients with Unknown Feedback Date | |
| Clients Escalated | |
| Avg. Days Since Last Contact | |
| Clients by Pillar | |

---

Always highlight overdue items first. Never leave Action Required blank — always provide a specific, actionable next step.
