# Network Playbooks

## Mail Egress Pinning (`mail_egress.yml`)

This playbook exists to make SMTP traffic from `vela-app60` consistently use
the dedicated public mail IP.

Why this is needed:

- Stalwart runs in containers and traffic traverses Docker/Swarm networking.
- With multiple host interfaces (`ens2` public, `ens5` private), Linux may pick
  a different return path than the ingress path unless policy routing is explicit.
- Asymmetric paths plus strict reverse-path filtering can cause SMTP timeouts.

What the playbook enforces:

- A dedicated routing table with default route via the public interface gateway.
- Policy rule: mail fwmark (`0x25`) -> dedicated routing table.
- Policy rule: source `public_ip/32` -> dedicated routing table.
- iptables mangle marks for SMTP/Submission/SMTPS/IMAPS flows
  (both destination and source port matches) so forward and reply directions are pinned.
- SNAT to the dedicated public IP for marked SMTP egress.
- `rp_filter=2` (loose mode) on relevant interfaces (`all`, `default`,
  public interface, docker bridge, and optional private interface).
- Persistence via systemd unit (`polaris-mail-egress.service`).

Gateway handling:

- `public_gateway` can be set explicitly.
- If omitted, the playbook auto-detects the gateway from:
  `ip -4 route show default dev <public_interface>`.

Operational goal:

- Keep inbound and outbound mail traffic symmetric on the dedicated IP,
  improve deliverability, and avoid accidental fallback to shared/private egress.
