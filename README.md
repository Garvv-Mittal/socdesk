# SOCDesk — Security Operations Help Desk Lab

A containerized SOC/help-desk simulation for practicing **security ticket triage, investigation, escalation, remediation, and documentation**.

Built with **osTicket, MySQL, Podman, and Linux**. All users, hosts, domains, and incidents are simulated.

---

## Overview

SOC analysts handle alerts and user-reported issues through ticketing systems. SOCDesk recreates that workflow locally with five simulated scenarios: phishing, account lockout, malware alert, unauthorized login, and VPN failure.

Each ticket follows this lifecycle:

```text
Creation → Triage → Priority → Assignment → Investigation
        → User Communication → Remediation / Escalation
        → Documentation → Closure
```

The goal is not just to deploy a ticketing app, but to practice the **operational workflow of handling security incidents**.

---

## Architecture

```text
User → osTicket (web app) ⇄ MySQL (persistent volume)
            │
            └→ Triage → Investigation → Remediation or Escalation (L2 SOC) → Closure
```

| Component | Technology |
|---|---|
| Ticketing | osTicket |
| Database | MySQL 5.7 |
| Runtime | Podman + Podman Compose |
| OS | Linux (Bazzite) |

---

## Setup

**Prerequisites:** Linux, Podman, Podman Compose, Git

```bash
git clone https://github.com/Garvv-Mittal/socdesk.git
cd socdesk
```

Create a `.env` file (never commit it):

```env
MYSQL_ROOT_PASSWORD=<your-root-password>
MYSQL_DATABASE=<your-database-name>
MYSQL_USER=<your-database-user>
MYSQL_PASSWORD=<your-database-password>
```

Start the lab:

```bash
podman-compose config     # validate
podman-compose up -d      # start
podman ps                 # expect socdesk_osticket_1 and socdesk_mysql_1
```

| Interface | URL |
|---|---|
| Support Center (users) | http://localhost:8080 |
| Staff Panel (analysts) | http://localhost:8080/scp/ |

**Useful commands**

```bash
podman logs socdesk_osticket_1    # osTicket logs
podman logs socdesk_mysql_1       # MySQL logs
podman-compose down               # stop (MySQL volume is retained)
```

---

## Investigation Workflow

1. **Intake** — user, contact, description, host, time, indicators, actions already taken
2. **Triage** — is it a security issue, what's the impact, what evidence exists, what priority
3. **Investigation** — separate *observed evidence* from *analyst assessment* so assumptions aren't reported as findings
4. **Remediation / Escalation** — fix what the evidence supports; escalate what it doesn't
5. **Documentation & Closure** — close only when evidence and remediation support it

---

## Simulated Scenarios

| # | Scenario | Priority | Key Actions | Outcome |
|---|---|---|---|---|
| 1 | **Phishing email** (fake Microsoft 365 password reset) | High | Flagged lookalike sender domain, urgency language, and suspicious URL; user confirmed no click; advised delete/report | Resolved |
| 2 | **Account lockout** | Medium | Confirmed temporary lockout from repeated failures; advised retry after lockout period | Resolved |
| 3 | **Malware alert** (`update.exe` in `C:\Users\Amit\Downloads\` on `WS-AMIT-042`) | Critical | Blocked execution, recommended quarantine/deletion via endpoint security, documented file and path | Resolved |
| 4 | **Unauthorized login** (02:31 AM from Singapore, user in Delhi, unexpected MFA prompt) | High | Treated as potential compromise (not confirmed); forced password change, reviewed sessions, told user not to approve MFA prompts | **Escalated to L2 SOC** |
| 5 | **VPN auth error** | Medium | Confirmed internet and account both worked; isolated issue to VPN client; restart, then update/reinstall | Resolved |

> Scenario 4 was deliberately left open: the evidence suggested compromise but didn't confirm it.

---

## Lab Results

| Metric | Result |
|---|---:|
| Tickets Opened | 7 |
| Tickets Assigned | 5 |
| Tickets Closed | 5 |
| Overdue / Reopened / Deleted | 0 |
| Service Time | 8.0 |
| Response Time | 10.0 |

---

## Lessons Learned

- Not every alert is a confirmed incident.
- Keep evidence and assumptions separate.
- Priority should reflect impact and urgency.
- Escalate instead of closing prematurely.
- Clear documentation is part of incident response.

---

## Roadmap

- [ ] Integrate a SIEM (Wazuh or Splunk) and ingest Windows Event Logs
- [ ] Automate alert-to-ticket creation
- [ ] Map incidents to MITRE ATT&CK
- [ ] Write detection and response playbooks
- [ ] Add Wireshark / Nmap investigation scenarios
- [ ] Build SOC metrics dashboards

---

## Disclaimer

Educational lab. All identities, hosts, domains, credentials, and incidents are simulated. No real systems or production credentials are involved.
