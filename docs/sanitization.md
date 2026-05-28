# Sanitization & Safe Publishing Guidelines
Status: Draft (to be updated during implementation)

## What I do NOT publish
- WAN (Wide Area Network — the public internet-facing side of the network) / public IPs, DDNS/domains, VPN details/keys
- Passwords, tokens, secrets
- Full firewall exports or backups that include sensitive data

## What IS okay to publish
- Representative RFC1918 (the standard defining private IP address ranges reserved for internal networks) subnets (10.x/172.16-31/192.168)
- Redacted screenshots that show architecture + policy logic
- Sanitized snippets (not full exports)

## Redaction checklist (before uploading)
- Blur WAN IP/gateway and external DNS names
- Remove port forwards and exposed management services
- Remove usernames if tied to personal identifiers
