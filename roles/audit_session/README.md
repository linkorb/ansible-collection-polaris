# audit_session role

Records interactive sessions with `tlog` and enables `auditd` with group-readable logs.

## Variables
- `audit_session_users` (list): users to switch shell to `tlog-rec-session` and place in `tlog/systemd-journal/audit` groups.
- `audit_session_shell_path` (string): default `/usr/bin/tlog-rec-session`.
- `audit_session_shell_binary` (string): default `/bin/bash`.
- `audit_session_tlog_notice` (string): banner shown when recording starts.
- `audit_session_writer` (string): `journal` (default) or `file`.
- `audit_session_set_audit_log_group` (bool): set `log_group = audit` in auditd.conf.
- `audit_session_extra_packages` (list): extra packages to install.

## Handlers
- `Restart auditd`
