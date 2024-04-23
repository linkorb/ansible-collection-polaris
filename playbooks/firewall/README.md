# Firewall

The firewall layer drops unwanted external traffic to a public interface whilst
permitting local and private traffic.  It can be used to firewall a Polaris
cluster running docker as long as docker nodes don't advertise the IP address
of the public interface.

## Variables

- `alias_of_public_network_interface` default: eth0

- `externally_accessible_network_ports` each item in this list is a port
  specification {"port": <int>, "proto": <string>}, e.g. {"port": 67, "proto":
'udp"}

- `log_dropped_network_traffic` default: false
