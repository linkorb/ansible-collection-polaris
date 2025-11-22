# promtail_host role

Installs and configures Promtail to ship host logs to Loki.

## Variables
- `promtail_loki_url` (string): Loki push endpoint.
- `promtail_loki_basic_auth` (dict): `{name, password}` for basic auth.
- `promtail_constellation_label` (string): optional label.
- `promtail_extra_labels` (dict): extra labels applied to all logs.

## Handlers
- `Restart promtail`
