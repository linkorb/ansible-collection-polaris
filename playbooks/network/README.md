# Network Playbooks

## Email Egress Pinning (`email_egress.yml`)

This playbook exists to keep `mail.linkorb.com` on one stable dedicated public
IP for inbound and reply-path symmetry, while also forcing outbound-initiated
SMTP to use that same IP.

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
- iptables connmark on new inbound connections targeting `public_ip`, then mark
  restore on reply packets, so traffic that arrived on the dedicated IP returns
  through the dedicated route regardless of application port.
- iptables mangle marks for outbound-initiated SMTP/Submission/SMTPS flows
  (`smtp_ports`, default `25,465,587`).
- iptables mangle source-port marks for SMTP ports.
- SNAT to the dedicated public IP for marked SMTP egress.
- `rp_filter=2` (loose mode) on relevant interfaces (`all`, `default`,
  public interface, docker bridge, and optional private interface).
- Persistence via systemd unit (`polaris-email-egress.service`).

Two-IP behavior:

- Hosts can serve traffic on both dedicated and non-dedicated public IPs.
- SMTP flows are always pinned to the dedicated path for reputation stability.
- Any flow that entered via the dedicated IP is pinned for its reply direction
  by connection mark, so no per-port HTTP(S) list is required.
- This keeps Outlook autodiscover replies on the mail IP while avoiding
  diversion of traffic that arrived on non-dedicated IPs.

Gateway handling:

- `public_gateway` can be set explicitly.
- If omitted, the playbook auto-detects the gateway from:
  `ip -4 route show default dev <public_interface>`.

Operational goal:

- Keep inbound and outbound email traffic symmetric on the dedicated IP.
- Preserve deliverability and sender reputation by avoiding accidental fallback
  to shared/private egress paths.

Quick verification on host:

- Check policy routing:
  `ip rule show | grep -E 'fwmark|from .*/32'`
- Check mangle marks:
  `iptables -t mangle -S | grep -E 'MARK --set-mark 0x25'`
- Confirm connection-mark flow pinning rules are active:
  `iptables -t mangle -S | grep -E 'CONNMARK --set-mark|connmark --mark'`
