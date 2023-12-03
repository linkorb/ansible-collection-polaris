### Variables

When importing a playbook from the Polaris collection you need to pass along as a variable the
`polaris` dictionary, which can have the following value properties set:

```yaml
- ansible.builtin.import_playbook: linkorb.polaris.layers
  vars:
    polaris:
      admins_active: # create users
        - username: alice
          fullname: Alice
      admins_removed: # remove users
        - bob
         
      docker_users: # Sudo-less docker usage
        - alice
         
-     #docker_registry_login_registry: https://index.docker.io/v1/
      #docker_registry_login_username: alice
      #docker_registry_login_password: secret

      #tailscale_authkey: secret
      #tailscale_args:
```

Refer to [the schema](./variables.schema.yaml) for further details.
