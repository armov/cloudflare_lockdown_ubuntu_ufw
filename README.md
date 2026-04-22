# cloudflare_lockdown_ubuntu_ufw

Hardens Ubuntu servers by restricting HTTP/HTTPS (80/443) access **exclusively to Cloudflare IP ranges** (IPv4 and IPv6) using UFW.

This role is designed for Cloudflare-proxied origins and is safe to run on
Ubuntu 22.04 / 24.04.

---

## What this role does

- Sets UFW defaults to `deny incoming`, `allow outgoing`
- Allows optional monitoring ports (e.g. Zabbix Agent)
- **Removes any existing broad `ALLOW Anywhere` rules for ports 80/443 (IPv4 and IPv6)**
- Allows Cloudflare IPv4 and IPv6 CIDR ranges to access ports 80/443
- Ensures IPv6 is handled correctly
- Disables legacy FERM firewall if present
- Enables UFW after rules are applied

---

## Important notes

- This role **intentionally removes** global `ALLOW IN Anywhere` rules for ports
  80/443 to enforce Cloudflare-only access.
- If your server must be reachable directly (non-Cloudflare traffic), this role
  is **not appropriate**.
- Cloudflare must be enabled (orange-cloud) for the protected hostnames,
  otherwise origin access will fail.

---

## Variables

### Core
- `ufw_cloudflare_ports` (default: `[80, 443]`)
- `ufw_cloudflare_ipv4`
- `ufw_cloudflare_ipv6`

### Optional
- `ufw_allow_zabbix_agent` (default: `true`)
- `ufw_zabbix_agent_port` (default: `10050`)
- `ufw_enable_ipv6` (default: `true`)

---

## Example usage

```yaml
- hosts: web
  become: true
  roles:
    - role: cloudflare_lockdown_ubuntu_ufw
