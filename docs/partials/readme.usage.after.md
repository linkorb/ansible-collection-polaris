### Variables

When importing a playbook from the Polaris collection you need to pass along as a variable the
`polaris` dictionary, which can have the following value properties set:

```yaml
- ansible.builtin.import_playbook: linkorb.polaris.layers
  vars:
    # All optional variables are commented
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
