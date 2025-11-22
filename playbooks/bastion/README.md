# Bastion

Records interactive sessions with `tlog` and enables `auditd` with group-readable logs (`log_group = audit`).

Logs are shipped to Loki.

Members of `polaris_admins_active` are placed in the `tlog/systemd-journal/audit` groups and their shell is switched to `tlog-rec-session` (bash, with the "journal" writer).
These admin users are permitted to sudo only when their username is in the list of `bastion_sudoers`.

A banner is shown to the user when recording starts:
```
ATTENTION! Your session is being recorded!
```

## Required Variables

- `bastion_loki_url`: URL of the Loki push endpoint
- `bastion_constellation_label`: name of the bastion constellation; used by promtail to label log streams
- `bastion_loki_user_name`: name of the user permitted to ship logs to Loki
- `bastion_loki_user_password`: password of the user permitted to ship logs to Loki

## Recommended Variables

- `bastion_sudoers`: List of usernames, matching those in `polaris_admins_active`, permitted to sudo

## Usage

```yaml
# site.yaml

- name: Bastion hosts are configured with bastion layers
  ansible.builtin.import_playbook: linkorb.polaris.bastion_layers
  vars:
    target_group: bastions
```
