# Network Playbooks

## Email Egress Pinning (`email_egress.yml`)

This playbook exists to keep all email-related traffic for `mail.linkorb.com`
on one stable dedicated public IP.

Why this is needed:

- Mail reputation checks depend on consistent SMTP source IP behavior.
- DNS identity alignment matters: forward DNS (A) and reverse DNS (PTR) for the
  mail host should consistently map to the same IP.
- Stalwart runs in containers and traffic traverses Docker/Swarm networking.
- With multiple host interfaces (`ens2` public, `ens5` private), Linux may pick
  a different return path than the ingress path unless policy routing is explicit.
- Asymmetric paths plus strict reverse-path filtering can cause SMTP timeouts.
- This host also exposes Outlook autodiscover over HTTP(S), so HTTP(S) reply
  traffic must stay pinned to the same dedicated path.

What the playbook enforces:

- A dedicated routing table with default route via the public interface gateway.
- Policy rule: email fwmark (`0x25`) -> dedicated routing table.
- Policy rule: source `public_ip/32` -> dedicated routing table.
- iptables mangle marks for SMTP/Submission/SMTPS/IMAPS destination flows.
- iptables mangle source-port marks for SMTP ports plus configured HTTP ports
  (`http_ports`, default `80/443`) so SMTP and HTTP(S) replies stay pinned.
- SNAT to the dedicated public IP for marked SMTP egress.
- `rp_filter=2` (loose mode) on relevant interfaces (`all`, `default`,
  public interface, docker bridge, and optional private interface).
- Persistence via systemd unit (`polaris-email-egress.service`).

Gateway handling:

- `public_gateway` can be set explicitly.
- If omitted, the playbook auto-detects the gateway from:
  `ip -4 route show default dev <public_interface>`.

Operational goal:

- Keep inbound and outbound email traffic symmetric on the dedicated IP.
- Preserve deliverability and sender reputation by avoiding accidental fallback
  to shared/private egress paths.
