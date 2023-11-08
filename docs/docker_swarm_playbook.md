# Docker Swarm Playbook

The docker_swarm playbook in this collection will initialise one or more swarms and joins manager and worker nodes to the swarm.

## How to use the Docker Swarm Playbook

### Prepare your Inventory

Group hosts in your inventory into two or three groups:

- a group of hosts that will each initialise a new Swarm
- a group of hosts that will join a Swarm as manager nodes
- a group of hosts that will join a Swarm as worker nodes

Declare a variable `join_swarm_initialised_by` for each host that will join a Swarm.  The value of the variable is the name or alias of the host which initialised the Swarm.  Your inventory may look something like this:

```ini
[my_primary_swarm_managers]
my-swarm-node-a1
my-swarm-node-b1

[my_secondary_swarm_managers]
my-swarm-node-a2 join_swarm_initialised_by=my-swarm-node-a1

[my_swarm_nodes]
my-swarm-node-a3 join_swarm_initialised_by=my-swarm-node-a1
my-swarm-node-b2 join_swarm_initialised_by=my-swarm-node-b1
```

### Call the playbook

When you call the playbook from your own playbook, map your group names to the ones used in the docker_swarm playbook:

```yaml
- name: Hosts belong to Docker Swarms
  vars:
    polaris_primary_swarm_managers: my_primary_swarm_managers
    polaris_secondary_swarm_managers: my_secondary_swarm_managers
    polaris_swarm_nodes: my_swarm_nodes
  import_playbook: linkorb.polaris.docker_swarm
```
