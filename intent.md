# Intent: results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (approved PRD N-17, 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by PRD N-17 and is used only as history; nothing in this intent is approved on its authority.

## Problem

- **I-1** The fictional clinic receives avoidable calls from patients checking whether their results are ready. PRD: "The fictional clinic receives avoidable calls from patients checking whether results are ready."
- **I-2** Patients have no signal that a reviewed result is waiting for them, so they call to ask about status rather than about clinical content.

## Proposed outcome

- **I-3** When a doctor has reviewed and released a result, the patient is told that an update is available and is directed to the existing authenticated portal to view it. PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal."
- **I-4** "Ready" means reviewed and released by a doctor, not that the laboratory upload finished. Product owner: "'Ready' must not mean that the laboratory upload merely finished."

### How success will be observed

- **S-1** Results-status calls go down against a measured baseline. Status-only calls are counted separately from clinical questions (support lead: "count status-only calls separately from clinical questions; compare against a measured baseline").
- **S-2** Notification delivery failures are tracked and handled without duplicate patient messages (PRD: "Measure reduction in results-status calls and delivery failures").
- **S-3** No numerical target and no baseline are approved yet. Both are to be agreed during discovery (PRD: "The baseline and numerical target will be agreed during discovery; none is approved yet."). The 90% one-week figure in the superseded request is not a target.

## Affected users and systems

- **Patients** receive the notification and open their result in the existing authenticated portal.
- **Doctors** review and release results; their release is what makes a notification eligible.
- **Support staff** handle the calls being reduced and provide the status-only vs clinical call counts.
- **Existing authenticated portal and its result route**: the destination for the patient; no redesign (engineering lead: "the existing portal path is adequate").
- **Notification delivery and its logs**: must prevent duplicates per released result version, handle delivery failures and keep messages and logs free of clinical content.

## Out of scope

The following are excluded from this intent:

- **X-1** Portal redesign (PRD scope).
- **X-2** A new or rebuilt patient dashboard: a separate project (engineering lead). Requested only by the superseded request.
- **X-3** Appointment scheduling (PRD scope).
- **X-4** Clinical interpretation of results (PRD scope).
- **X-5** Notifying third parties (PRD scope). The referring doctor is not approved as a recipient; that question is tracked separately as OPEN-2.
- **X-6** Notifying on lab upload, putting the test result in an SMS, or any attached results or public result links: all come from the superseded request or are forbidden by the PRD.

## Constraints

- **C-1** Review before notification: a doctor must review and release the result before any notification is eligible (PRD; product owner keeps this gate).
- **C-2** Message content: the notification only says that an update is available and points to the existing authenticated portal. No diagnosis, test name or result, clinical measurement or treatment detail in the message or in notification and operational logs (PRD; privacy owner: "no sensitive content in messages or operational logs").
- **C-3** Access only through the existing authenticated portal. No attached results and no public result link (PRD).
- **C-4** No duplicate notifications for the same released result version; delivery failures are handled without duplicate patient messages (PRD).
- **C-5** No recipient other than the patient is approved. The referring doctor must not be treated as approved because the old request asked for it (clinical owner: "do not infer approval for referring-doctor notifications from the old request").
- **C-6** All communication must be fully WCAG 2.2 compliant if the communication channel allows it.

## Open questions

- **OPEN-1 Channel** (SMS, email or portal push). Owners: **product owner** and **privacy owner**, decided after the consent and delivery review. Still under review per the privacy owner. Not decided by this intent.
- **OPEN-2 Referring-doctor notification**: whether the referring doctor may be notified and under what recipient authorization. Owners: **clinical owner** and **privacy owner**. Not approved by the PRD and not approved by this intent.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
