# promtail_host role

Installs and configures Promtail to ship host logs to Loki.

## Variables
- `promtail_loki_url` (string, required): Loki push endpoint (pass in from inventory/vars).
- `promtail_loki_basic_auth` (dict): `{name, password}` for basic auth.
- `promtail_constellation_label` (string): optional label.
- `promtail_extra_labels` (dict): extra labels applied to all logs.

You must supply `promtail_loki_url` (e.g., via inventory or extra vars); it is not set by default in this public collection.

## Handlers
- `Restart promtail`
