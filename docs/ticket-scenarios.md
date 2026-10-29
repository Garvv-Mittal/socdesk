# SOCDesk Ticket Scenarios

Five simulated tickets, each run through the workflow in [investigation-workflow.md](investigation-workflow.md). All identities, hosts, and domains are fictional.

| # | Scenario | Priority | Status |
|---|---|---|---|
| 1 | Phishing Email | High | Resolved |
| 2 | Account Lockout | Medium | Resolved |
| 3 | Malware Alert | Critical | Resolved |
| 4 | Unauthorized Login | High | Open / Escalated |
| 5 | VPN Connectivity | Medium | Resolved |

---

## 1. Phishing Email: High, Resolved

**Reporter:** Rahul Sharma

**Report:** A security-themed email arrived from `security-alert@micr0soft-support.com` with a link to `https://microsoft-account-verification.example.com`. The user did not click it.

**Indicators:** lookalike domain (`micr0soft`), brand impersonation, urgent verification language, suspicious URL.

**Assessment:** Classified as phishing. The URL was treated as suspicious but not verified as malicious.

**Response:** Do not click or enter credentials, delete/report the email, report similar messages in future.

---

## 2. Account Lockout: Medium, Resolved

**Reporter:** Priya Mehta (`pmehta`), working remotely, first noticed at 10:15 AM.

**Evidence:** Multiple failed logins, then lockout. No evidence of compromise.

**Assessment:** Treated as a routine lockout, not a compromise.

**Response:** Wait out the lockout (12 hours in this scenario), verify credentials, retry, and contact support if still locked.

---

## 3. Malware Alert: Critical, Resolved

**Reporter:** Amit Verma, host `WS-AMIT-042`.

**Report:** After downloading a PDF viewer from an external site, endpoint security flagged `C:\Users\Amit\Downloads\update.exe`.

**Assessment:** Potentially malicious executable. It was not executed during the investigation.

**Response:**
1. Do not execute the file.
2. Quarantine or remove it via the endpoint security workflow.
3. Follow the malware-remediation procedure.
4. Monitor the endpoint for further indicators.

---

## 4. Unauthorized Login: High, Open / Escalated

**Reporter:** Neha Kapoor (`nkapoor`).

**Report:** Login at 02:31 AM from Singapore; the user is expected in Delhi. She denies the login and received an unexpected MFA prompt.

**Indicators:** unrecognized login, geographic mismatch, user denial, unsolicited MFA.

**Assessment:** Potential unauthorized access. The evidence justifies escalation but does not confirm compromise.

**Response:** Change to a strong unique password, don't approve unexpected MFA requests, review active sessions and recent authentication activity, escalate to L2 SOC.

**Why it stays open:** Closing a possible compromise on thin evidence is the mistake this lab is designed to avoid.

---

## 5. VPN Connectivity: Medium, Resolved

**Reporter:** Arjun Singh, device `LAPTOP-ARJUN-17`.

**Report:** "Authentication failed" on the corporate VPN. Internet and corporate credentials both work.

**Assessment:** Likely a VPN client or VPN-specific issue (outdated client, bad config, or temporary auth problem).

**Response:** Restart the client and retry, then update it, then reinstall or reconfigure if needed.
