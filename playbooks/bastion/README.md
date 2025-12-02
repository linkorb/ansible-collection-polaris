# Bastion

Records interactive sessions with `tlog` and enables `auditd` with group-readable logs (`log_group = audit`).

Logs are shipped to Loki.

Members of `polaris_admins_active` are placed in the `tlog/systemd-journal/audit` groups and their shell is switched to `tlog-rec-session` (bash, with the "journal" writer).

A banner is shown to the user when recording starts:
```
ATTENTION! Your session is being recorded!
```

## Required Variables

- `bastion_loki_url`: URL of the Loki push endpoint
- `bastion_constellation_label`: name of the bastion constellation; used by promtail to label log streams
- `bastion_loki_user_name`: name of the user permitted to ship logs to Loki
- `bastion_loki_user_password`: password of the user permitted to ship logs to Loki
