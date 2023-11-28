# Polaris Collection for Ansible
<!-- Add CI and code coverage badges here. Samples included below. -->
[![CI](https://github.com/linkorb/ansible-collection-polaris/workflows/CI/badge.svg?event=push)](https://github.com/linkorb/ansible-collection-polaris/actions) [![Codecov](https://img.shields.io/codecov/c/github/linkorb/ansible-collection-polaris)](https://codecov.io/gh/linkorb/ansible-collection-polaris)

<!-- Describe the collection and why a user would want to use it. What does the collection do? -->

## Our mission

<!-- Put your collection project's mission statement in here. -->

## Contributing to this collection

<!--Describe how the community can contribute to your collection. At a minimum, fill up and include the CONTRIBUTING.md file containing how and where users can create issues to report problems or request features for this collection. List contribution requirements, including preferred workflows and necessary testing, so you can benefit from community PRs. If you are following general Ansible contributor guidelines, you can link to - [Ansible Community Guide](https://docs.ansible.com/ansible/devel/community/index.html). List the current maintainers (contributors with write or higher access to the repository). The following can be included:-->

The content of this collection is made by people like you, a community of individuals collaborating on making the world better through developing automation software.

We are actively accepting new contributors and all types of contributions are very welcome.

Don't know how to start? Refer to the [Ansible community guide](https://docs.ansible.com/ansible/devel/community/index.html)!

Want to submit code changes? Take a look at the [Quick-start development guide](https://docs.ansible.com/ansible/devel/community/create_pr_quick_start.html).

We also use the following guidelines:

* [Collection review checklist](https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_reviewing.html)
* [Ansible development guide](https://docs.ansible.com/ansible/devel/dev_guide/index.html)
* [Ansible collection development guide](https://docs.ansible.com/ansible/devel/dev_guide/developing_collections.html#contributing-to-collections)

## Tested with Ansible

<!-- List the versions of Ansible the collection has been tested with. Must match what is in galaxy.yml. -->

## External requirements

<!-- List any external resources the collection depends on, for example minimum versions of an OS, libraries, or utilities. Do not list other Ansible collections here. -->

## Included content

<!-- Galaxy will eventually list the module docs within the UI, but until that is ready, you may need to either describe your plugins etc here, or point to an external docsite to cover that information. -->

## Using this collection

<!--Include some quick examples that cover the most common use cases for your collection content. It can include the following examples of installation and upgrade: -->

### Installing the Collection from Ansible Galaxy

Before using this collection, you need to install it with the Ansible Galaxy command-line tool:
```bash
ansible-galaxy collection install linkorb.polaris
```

You can also include it in a `requirements.yml` file and install it with `ansible-galaxy collection install -r requirements.yml`, using the format:
```yaml
---
collections:
  - name: linkorb.polaris
```

Note that if you install the collection from Ansible Galaxy, it will not be upgraded automatically when you upgrade the `ansible` package. To upgrade the collection to the latest available version, run the following command:
```bash
ansible-galaxy collection install linkorb.polaris --upgrade
```

You can also install a specific version of the collection, for example, if you need to downgrade when something is broken in the latest version (please report an issue in this repository). Use the following syntax to install version `0.1.0`:

```bash
ansible-galaxy collection install linkorb.polaris:==0.1.0
```

See [using Ansible collections](https://docs.ansible.com/ansible/devel/user_guide/collections_using.html) for more details.

### Organise your Inventory

```ini
[polaris_hosts]
the-monitor
the-swarm-manager
swarm-node-1
swarm-node-2
swarm-node-3

[polaris_primary_swarm_managers]
the-swarm-manager

[polaris_swarm_nodes]
swarm-node-1 join_swarm_initialised_by=the-swarm-manager
swarm-node-2 join_swarm_initialised_by=the-swarm-manager
swarm-node-3 join_swarm_initialised_by=the-swarm-manager
```

Organise your Inventory into a group named "`polaris_hosts`".

Place hosts that will be manager Docker Swarm nodes into a group named "`polaris_primary_swarm_managers`".  A Docker Swarm will be initialised on each host in this group.

Place hosts that will be Docker Swarm nodes into a group named "`polaris_swarm_nodes`".  Provide the host var named "`join_swarm_initialised_by`" for each host in this group.  The node will join the swarm initialised on the named host.

## Release notes

See the [changelog](https://github.com/linkorb/ansible-collection-polaris/tree/main/CHANGELOG.rst).
