# Configuration Examples

This directory is for sanitized configuration examples that support the documentation without exposing secrets or operational identifiers.

## Good candidates

- Example Corosync snippets with generic or lab-only values
- Storage configuration examples with credentials removed
- Firewall examples that do not expose sensitive public infrastructure
- Example HA resource definitions
- Educational configuration snippets using obvious placeholders

## Never commit

- Passwords or operational usernames
- Private or public SSH keys
- Private or public WireGuard keys
- AWS access keys, account identifiers or temporary credentials
- API keys or tokens
- Cookies or session IDs
- Secret environment files such as real `.env` files
- Public IP addresses or real public endpoints
- Backup files containing credentials

When in doubt, replace a sensitive or operational value with an obvious placeholder such as:

```text
<REDACTED>
<EXAMPLE_IP>
<USERNAME_REMOVED>
<KEY_REMOVED>
<TOKEN_REMOVED>
<ENDPOINT_REMOVED>
```

The goal is to show understanding of configuration structure, not to publish operational details from the environment.
