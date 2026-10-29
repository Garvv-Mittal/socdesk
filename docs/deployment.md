# SOCDesk Deployment Guide

Local deployment on Linux with Podman. Developed and tested on Bazzite. For the quick start, see the main [README](../README.md).

## Prerequisites

Podman, podman-compose, Git, a web browser.

```bash
podman --version
podman-compose --version
```

## Deploy

```bash
git clone <repository-url>
cd socdesk
nano .env                 # see below; never commit this file
podman-compose config     # validate YAML and env references
podman-compose up -d      # start detached
podman ps                 # expect socdesk_osticket_1 and socdesk_mysql_1
```

`.env`:

```env
MYSQL_ROOT_PASSWORD=your_root_password
MYSQL_DATABASE=osticket
MYSQL_USER=osticket
MYSQL_PASSWORD=your_database_password
```

Access:

- Support Center: http://localhost:8080
- Staff panel: http://localhost:8080/scp/

## Day-to-Day Commands

```bash
podman-compose down         # stop (volume is kept)
podman-compose up -d        # start
podman-compose restart
podman-compose logs -f      # follow logs
podman logs socdesk_osticket_1
podman logs socdesk_mysql_1
podman volume ls
```

## Reset the Lab

This permanently deletes all ticket data.

```bash
podman-compose down
podman volume ls                  # find the MySQL volume
podman volume rm <volume-name>
podman-compose up -d
```

## Troubleshooting

| Problem | Check |
|---|---|
| Containers not running | `podman ps -a`, then read both container logs |
| osTicket can't reach MySQL | Both containers running? `MYSQL_HOST` must be `mysql`, not `localhost`; confirm `.env` values |
| Port 8080 in use | `ss -tulpn \| grep 8080`, then change the mapping to `"8081:80"` in `docker-compose.yml` |

## Verification Checklist

- [ ] Both containers running
- [ ] Portal and staff panel load
- [ ] A ticket can be created, assigned, and resolved
- [ ] Data survives `podman-compose down` followed by `up -d`
