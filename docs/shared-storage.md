# Shared Storage with TrueNAS

## Role in the lab

TrueNAS provides storage that can be reached by the Proxmox VE cluster. The key idea is that a VM disk placed on shared storage is not tied to only one physical Proxmox host.

<p align="center">
  <img src="../assets/architecture-overview.svg" alt="Shared storage architecture" width="900">
</p>

## Why it matters

Shared storage is important for this homelab because it supports:

- VM migration between cluster nodes
- Faster live migration when the virtual disk does not need to be recopied
- HA recovery, because another node can access the same VM disk
- Centralized storage management

## Current setup

| System | Purpose |
|---|---|
| TrueNAS | Shared NFS storage for the Proxmox lab |

The real storage endpoint and network addressing are intentionally omitted from the public repository.

## Practical observation

A migration involving local disk storage took noticeably longer because disk data had to be transferred between hosts. Once the VM used shared storage, migration was much faster.

That distinction is important: **cluster membership alone does not make storage shared**.

## Validation

From a Proxmox node:

```bash
pvesm status
```

The sanitized validation snapshot confirms that the TrueNAS NFS storage was active at capture time.

## Design considerations

The current lab is intentionally simple. Future improvements may include a dedicated storage network to separate storage traffic from management and cluster communication.

## Security note

The public repository does not include:

- Storage IP addresses
- Export paths that are not needed for explanation
- Usernames or passwords
- Authentication secrets
- Private keys
- Sensitive storage configuration
