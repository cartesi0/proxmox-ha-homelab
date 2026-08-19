# Cluster Validation

This page is reserved for **sanitized evidence from the real lab**. It intentionally avoids invented command output.

## Commands to capture

Run these commands on a Proxmox VE node:

```bash
pvecm status
pvecm nodes
pvesm status
ha-manager status
```

Optional service checks:

```bash
systemctl status corosync --no-pager
systemctl status pve-cluster --no-pager
systemctl status pve-ha-lrm --no-pager
systemctl status pve-ha-crm --no-pager
```

## What the evidence should demonstrate

- Three cluster members are visible
- Quorum is present
- Shared storage is available
- HA services are healthy
- HA resources show the expected runtime location

## Before publishing output

Review every line and remove or redact anything that should not be public, including:

- Passwords
- API tokens
- Private keys
- Session cookies
- Public IP addresses when unnecessary
- Internal hostnames or identifiers that add no portfolio value
- Usernames or paths that expose personal information

Private RFC1918 lab addresses may be kept when they help explain the topology, but they are not proof of security by themselves.

## Evidence template

```text
$ pvecm status
[REAL SANITIZED OUTPUT TO BE ADDED]

$ pvecm nodes
[REAL SANITIZED OUTPUT TO BE ADDED]

$ pvesm status
[REAL SANITIZED OUTPUT TO BE ADDED]

$ ha-manager status
[REAL SANITIZED OUTPUT TO BE ADDED]
```

The placeholders above are deliberate. Real output will be added only after it has been collected from the lab and reviewed.
