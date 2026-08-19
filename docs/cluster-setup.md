# Proxmox VE Cluster Setup

## Purpose

This lab uses three Proxmox VE hosts in a single cluster to study centralized management, Corosync, quorum, migration and HA behavior.

## Nodes

| Node | Management IP | Role |
|---|---|---|
| Proxmox Node 1 | `192.168.1.47` | Cluster node |
| Proxmox Node 2 | `192.168.1.48` | Cluster node |
| Proxmox Node 3 | `192.168.1.49` | Cluster node |

## Cluster communication

Proxmox VE uses Corosync for cluster membership and messaging. Reliable node-to-node connectivity is essential because quorum and HA decisions depend on the cluster having a consistent view of which nodes are available.

## Quorum

With three voting nodes, the cluster normally requires two votes to remain quorate. This makes the lab useful for testing the difference between losing one node and losing too many votes to safely operate.

```bash
pvecm status
pvecm nodes
```

These commands are used to check quorum, expected votes and current members.

## Corosync inspection

```bash
cat /etc/pve/corosync.conf
systemctl status corosync
systemctl status pve-cluster
```

Configuration changes should be made cautiously because an incorrect Corosync setup can affect the whole cluster.

## Shared storage check

TrueNAS provides the shared storage used by the lab.

```bash
pvesm status
```

Shared VM disks allow migrations between nodes without copying the entire virtual disk each time.

## Migration test

A VM was successfully migrated between active nodes. Migration was much faster when its disk was already on shared storage because only the runtime state needed to move rather than the whole disk image.

## HA test

A VM was added as a Proxmox HA resource. The node hosting that VM was then powered off to simulate a physical host failure.

Observed behavior:

1. The cluster detected the unavailable host.
2. HA selected another available node.
3. The VM was started on the surviving node.
4. When the failed host returned, it rejoined the cluster; the VM did not automatically move back simply because the original host was available again.

## Live migration vs HA failover

| Scenario | Source host | VM behavior | Typical use |
|---|---|---|---|
| Live migration | Healthy | Keeps running while state moves | Maintenance / planned relocation |
| HA failover | Failed | Restarts on another node | Unexpected host failure |

## Useful commands

```bash
# Cluster
pvecm status
pvecm nodes

# Services
systemctl status corosync
systemctl status pve-cluster
systemctl status pve-ha-lrm
systemctl status pve-ha-crm

# Storage
pvesm status

# Workloads
qm list
pct list

# HA
ha-manager status
```

## Tests completed

- [x] Three-node cluster created
- [x] Nodes joined to the same cluster
- [x] Cluster communication verified
- [x] Shared storage configured
- [x] VM migration tested
- [x] Live migration tested
- [x] HA resource configured
- [x] Physical node failure simulated
- [x] Automatic VM restart verified
- [x] Failed node returned to cluster

## Next validation step

The repository will later include sanitized real output from:

```bash
pvecm status
pvecm nodes
pvesm status
ha-manager status
```

Only output that has been reviewed for secrets or unnecessary internal details will be published.
