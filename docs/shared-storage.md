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

## Current endpoint

| System | Address | Purpose |
|---|---:|---|
| TrueNAS | `192.168.10.76` | Shared VM storage |

## Practical observation

A migration involving local disk storage took noticeably longer because disk data had to be transferred between hosts. Once the VM used shared storage, migration was much faster.

That distinction is important: **cluster membership alone does not make storage shared**.

## Validation

From a Proxmox node:

```bash
pvesm status
```

Additional checks depend on the storage protocol in use and will be documented only after they are verified in the live lab.

## Design considerations

The current lab is intentionally simple. Future improvements may include a dedicated storage network to separate storage traffic from management and cluster communication.

## Security note

No credentials, authentication secrets, private keys or sensitive storage configuration should be committed to this public repository.
