# atproto dev environment

[![Deploy on exe.dev](https://raw.githubusercontent.com/boldsoftware/exe.dev/main/assets/buttons/deploy-on-exe-dev.png)](https://exe.dev/new?repo=https://tangled.org/katherine.app/atdev)

Self-contained [AT Protocol](https://atproto.com) development environment.

- PDS
- PLC
- Relay
- Jetstream
- Constellation

## setup

```sh
cp .env.example .env
```

Fill out `.env`. Set `DEPLOY_*` to `0` to disable a service.

## remote

| Service       | Address                         |
| ------------- | ------------------------------- |
| PDS           | `https://pds.$DOMAIN`           |
| PLC           | `https://plc.$DOMAIN`           |
| Relay         | `https://relay.$DOMAIN`         |
| Jetstream     | `https://jetstream.$DOMAIN`     |
| Constellation | `https://constellation.$DOMAIN` |

Caddy sits in front as a reverse proxy.

1. Set `DOMAIN` and fill in every blank secret in `.env`.
2. Point `pds`, `plc`, `relay`, `jetstream`, and `constellation` DNS records
   at this host and terminate TLS in front of Caddy (it listens on `CADDY_PORT`).
3. `docker compose up -d`.

Request crawl:

```sh
docker compose exec -T relay_db psql -U "$RELAY_POSTGRES_USER" -d "$RELAY_DATABASE" -c \
  "INSERT INTO host (created_at, updated_at, hostname, no_ssl, account_limit, trusted, status, last_seq, account_count) VALUES (now(), now(), 'pds.$DOMAIN', true, 100, false, 'active', -1, 0) ON CONFLICT (hostname) DO UPDATE SET no_ssl=true, status='active', updated_at=now();"

docker compose restart relay
```

## localhost

| Service       | Address                 |
| ------------- | ----------------------- |
| PDS           | `http://localhost:3000` |
| PLC           | `http://localhost:4000` |
| Relay         | `http://localhost:2470` |
| Jetstream     | `http://localhost:8080` |
| Constellation | `http://localhost:6789` |

```sh
docker compose -f compose.yaml -f compose.local.yaml up -d
```

Request crawl:

```sh
docker compose exec -T relay_db psql -U "$RELAY_POSTGRES_USER" -d "$RELAY_DATABASE" -c \
  "INSERT INTO host (created_at, updated_at, hostname, no_ssl, account_limit, trusted, status, last_seq, account_count) VALUES (now(), now(), 'localhost:3000', true, 100, false, 'active', -1, 0) ON CONFLICT (hostname) DO UPDATE SET no_ssl=true, status='active', updated_at=now();"

docker compose restart relay
```

## notes

- Handles use `.test` by default, e.g. `alice.test`.
- Constellation duplicates indexed records every every 4 seconds 🤷‍♀️.
