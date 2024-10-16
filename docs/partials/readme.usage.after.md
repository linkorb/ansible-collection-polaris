### Variables

Set the parameters of the collection by defining inventory variables.
A good way to do this is to set them in `group_vars/polaris_hosts/foo.yaml`.
By convention the variables understood by the collection all begin with "polaris_".

```yaml
# group_vars/polaris_hosts/vars.yaml

#polaris_admins_active: # create users
# - username: alice
#   fullname: Alice
#   pubkey: ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBZl3lQTRGhD5mdGgFEVuX+CAnTMz9MuY+f4vE2cqk9G alice@host
#polaris_admins_removed: # remove users
#  - bob

#polaris_docker_users: # Sudo-less docker usage
#  - alice

polaris_docker_swarm_advertise_address: secret
polaris_docker_registry_login: alice
polaris_docker_registry_pat: "{{ vault_polaris_docker_registry_pat }}"

#polaris_tailscale_args:

#polaris_traefik_tls_files:
#  - certFile: /path/to/sops/encrypted/cert
#    keyFile: /path/to/sops/encrypted/key
#    domain: example.com
#polaris_traefik_log_level: DEBUG
```

Refer to [the schema](./variables.schema.yaml) for further details.
