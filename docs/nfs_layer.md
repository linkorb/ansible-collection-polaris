# NFS Layer

The NFS layer is implemented as `linkorb.polaris.nfs.server` and is included from
the main `layers.yml` playbook. It is intentionally a layer playbook rather than a
role, because it represents a complete host layer that is applied once to hosts in
`polaris_nfs_hosts`.

Key design decisions:

- The layer configures plain Linux NFS exports with `nfs-kernel-server`; it does
  not provision distributed storage.
- The inventory must define the dedicated data disk explicitly with
  `polaris_nfs_data_disk`. The layer does not guess which block device is safe to
  format.
- The data disk is formatted as ext4 and mounted at `polaris_nfs_data_mount_point`.
- Exports are written to `/etc/exports.d/polaris-nfs.exports` from
  `polaris_nfs_exports`; the inventory owns the client networks and export
  options.
- Backups are optional. When `polaris_nfs_backup_enabled` is true, the inventory
  must provide the restic repository URL, source path, schedule, region, and
  credentials.

Inventory example:

```yaml
polaris_nfs_hosts:
  hosts:
    nfs1:
      polaris_nfs_data_disk: /dev/sdb
      polaris_nfs_data_mount_point: /data
      polaris_nfs_export_paths:
        - /data/nfs-export
        - /data/nfs-export/config
      polaris_nfs_exports:
        - path: /data/nfs-export
          clients: 192.0.2.0/24
          options: rw,sync,crossmnt,no_subtree_check,no_root_squash,fsid=0
        - path: /data/nfs-export/config
          clients: 192.0.2.0/24
          options: rw,sync,no_subtree_check,no_root_squash
      polaris_nfs_backup_enabled: true
      polaris_nfs_backup_repository: s3:https://s3.example.net/example-bucket/restic
      polaris_nfs_backup_source: /data/nfs-export
      polaris_nfs_backup_aws_default_region: example-region
      polaris_nfs_backup_calendar: "*-*-* 00:17:00"
      polaris_nfs_backup_restic_password: "{{ vault_nfs_backup_restic_password }}"
      polaris_nfs_backup_aws_access_key_id: "{{ vault_nfs_backup_aws_access_key_id }}"
      polaris_nfs_backup_aws_secret_access_key: "{{ vault_nfs_backup_aws_secret_access_key }}"
```
