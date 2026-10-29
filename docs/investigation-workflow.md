# SOCDesk Investigation Workflow

A structured process for handling security and IT-support tickets, from intake to closure.

```text
Intake → Triage & Classification → Priority → Evidence Review → Investigation
      → Remediation or Escalation → Documentation → User Response → Resolution → Closure
```

## 1. Intake

Capture: user, username/email, device, timestamp, issue description, indicators, and actions the user already took.

## 2. Triage and Classification

Decide what was reported and whether it is a security issue. Don't assume a confirmed incident; let the evidence set the next step. Categories used: Phishing, Account Lockout, Malware Alert, Unauthorized Login, VPN Connectivity.

## 3. Priority

| Priority | Example |
|---|---|
| Low | Minor technical issue |
| Medium | Account lockout, VPN problem |
| High | Phishing, suspicious login |
| Critical | Potential malware execution |

Priority reflects potential impact and urgency.

## 4. Evidence and Investigation

Record what is relevant to the scenario:

- **Email:** sender, recipient, subject, URL
- **Endpoint:** hostname, file name, file path
- **Authentication:** timestamp, location, MFA activity, username

Keep three levels separate so assumptions never read as findings:

```text
Observed fact → Suspicious indicator → Potential conclusion
```

Example: *unexpected login + user denies it + unsolicited MFA* gives **potential unauthorized access**, not confirmed compromise.

## 5. Remediation

| Scenario | Actions |
|---|---|
| Phishing | Don't click, don't enter credentials, report/delete |
| Malware | Don't execute, quarantine/remove via endpoint tooling |
| Lockout | Wait out lockout, verify credentials, retry |
| Unauthorized login | Change password, review sessions and auth events, refuse unexpected MFA, escalate |
| VPN | Restart client, retry, update/reinstall |

## 6. Escalation

Escalate instead of closing when:

- Evidence suggests possible compromise
- Security impact is uncertain
- The issue exceeds the current support level

*Example:* the unauthorized-login ticket stays open and goes to L2 SOC because the user denied the login and got an unexpected MFA prompt.

## 7. Documentation

Every ticket should record what was reported, observed, and done, what remains uncertain, and whether it was escalated. Another analyst should be able to pick it up without repeating the investigation.

## 8. User Communication

Give clear, actionable instructions without exposing unnecessary internal detail. For example: *"Please change your password and do not approve unexpected MFA prompts."*

## 9. Resolution and Closure

Resolve when remediation is complete, the user has instructions, and the notes are recorded. Resolution doesn't mean the investigation is finished; a suspicious-login ticket can stay open and escalated.

## Quick Checklist

- [ ] Intake: user, device, timestamp, issue recorded
- [ ] Triage: classified, priority set, indicators identified
- [ ] Investigation: facts separated from assumptions, escalation decision made
- [ ] Response: remediation given, affected accounts/devices protected
- [ ] Documentation: actions, outcome, and open questions recorded
- [ ] Closure: only when evidence and remediation support it
