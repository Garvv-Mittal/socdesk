# SOCDesk Architecture

SOCDesk runs a ticketing app (osTicket) and its database (MySQL) as two containers on one Podman network, hosted on Bazzite (Linux).

```text
Browser ──:8080──▶ osTicket container (:80) ──▶ MySQL container
                                                    │
                                              mysql_data volume
```

## Components

| Component | Image | Role | Exposure |
|---|---|---|---|
| osTicket | `devinsolutions/osticket:latest` | Ticket intake, triage, assignment, notes, escalation, closure | Host `8080` → container `80` |
| MySQL | `mysql:5.7` | Stores tickets, users, staff, departments, settings | Internal network only |

**Why Podman:** Bazzite is an immutable Fedora-based OS where installing Docker is not the preferred workflow. Podman runs rootless and works with Compose files.

## Networking

osTicket reaches the database by service name (`MYSQL_HOST: mysql`), not `localhost`. MySQL is not published to the host because only osTicket needs it.

## Persistence

```yaml
volumes:
  - mysql_data:/var/lib/mysql
```

The named volume keeps ticket data across container restarts and recreation.

## Security Notes

- Credentials live in `.env`, which is excluded via `.gitignore`.
- MySQL is not exposed to the host.
- The lab is local-only, and all incidents and identities are simulated.
