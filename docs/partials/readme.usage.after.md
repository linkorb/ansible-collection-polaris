### Variables

Set the parameters of the collection by defining inventory variables.
A good way to do this is to set them in `group_vars/polaris_hosts/foo.yaml`.

```yaml
# group_vars/polaris_hosts/vars.yaml
polaris:
  #admins_active: # create users
  # - username: alice
  #   fullname: Alice
  #   pubkey: ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBZl3lQTRGhD5mdGgFEVuX+CAnTMz9MuY+f4vE2cqk9G alice@host
  #admins_removed: # remove users
  #  - bob

  #docker_users: # Sudo-less docker usage
  #  - alice

  docker_swarm_advertise_address: secret
  docker_registry_login: alice
  docker_registry_pat: secret

  #tailscale_args:

  #traefik_tls_files:
  #  - certFile: /path/to/sops/encrypted/cert
  #    keyFile: /path/to/sops/encrypted/key
  #    domain: example.com
  #traefik_log_level: DEBUG
```

Refer to [the schema](./variables.schema.yaml) for further details.
