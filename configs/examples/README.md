# Configuration Examples

This directory is for sanitized configuration examples that support the documentation without exposing secrets.

## Good candidates

- Example Corosync snippets with generic or lab-only values
- Storage configuration examples with credentials removed
- Firewall examples that do not expose sensitive public infrastructure
- Example HA resource definitions

## Never commit

- Passwords
- Private SSH keys
- WireGuard private keys
- API tokens
- Cookies or session IDs
- Secret environment files
- Backup files containing credentials

When in doubt, replace a sensitive value with an obvious placeholder such as:

```text
<REDACTED>
<EXAMPLE_IP>
<USERNAME>
```

The goal is to show understanding of configuration structure, not to publish operational secrets.
