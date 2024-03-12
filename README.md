<!-- Managed by https://github.com/linkorb/repo-ansible. Manual changes will be overwritten. -->
ansible-collection-polaris
============






## Usage

Before using this collection, you need to install it with the Ansible Galaxy command-line tool:

```shell
ansible-galaxy collection install linkorb.polaris
```

You can also include it in a `requirements.yml` file and install it with `ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: linkorb.polaris
```
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
         
-     #docker_registry: https://ghcr.io/
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

## Contributing

We welcome contributions to make this repository even better. Whether it's fixing a bug, adding a feature, or improving documentation, your help is highly appreciated. To get started, fork this repository then clone your fork.

Be sure to familiarize yourself with LinkORB's [Contribution Guidelines](/CONTRIBUTING.md) for our standards around commits, branches, and pull requests, as well as our [code of conduct](/CODE_OF_CONDUCT.md) before submitting any changes.

If you are unable to implement changes you like yourself, don't hesitate to open a new issue report so that we or others may take care of it.
## Brought to you by the LinkORB Engineering team

<img src="http://www.linkorb.com/d/meta/tier1/images/linkorbengineering-logo.png" width="200px" /><br />
Check out our other projects at [linkorb.com/engineering](http://www.linkorb.com/engineering).
By the way, we're hiring!
