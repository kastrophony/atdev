# cloudflare dns tls

Overlay: TLS-terminating Caddy

Builds Caddy with the caddy-dns/cloudflare plugin and serves the custom
domains over HTTPS

From repo root

```bash
docker compose -f compose.yaml -f examples/cloudflare-tls/compose.cf.yaml up -d
```
