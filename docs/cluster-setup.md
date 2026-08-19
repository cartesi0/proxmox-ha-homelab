# Proxmox VE Cluster Setup

## Purpose

This lab uses three Proxmox VE hosts in a single cluster to study centralized management, Corosync, quorum, migration and HA behavior.

## Nodes

| Node | Management IP | Role |
|---|---|---|
| Proxmox Node 1 | `192.168.1.47` | Cluster node |
| Proxmox Node 2 | `192.168.1.48` | Cluster node |
| Proxmox Node 3 | `192.168.1.49` | Cluster node |

The real validation snapshot captured on 19 August 2026 reports the cluster name as `labcluster`, with all three nodes visible and quorum present.

<p align="center">
  <img src="../assets/cluster-status.svg" alt="Real cluster status" width="950">
</p>

## Cluster communication

Proxmox VE uses Corosync for cluster membership and messaging. Reliable node-to-node connectivity is essential because quorum and HA decisions depend on the cluster having a consistent view of which nodes are available.

The captured cluster output reports:

- Transport: `knet`
- Secure authentication: `on`
- Nodes: `3`
- Expected votes: `3`
- Total votes: `3`
- Quorum: `2`
- Quorate: `Yes`

## Quorum

With three voting nodes, the captured cluster state shows a quorum threshold of two votes. This makes the lab useful for testing the difference between losing one node and losing too many votes to safely operate.

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

In the real captured output, the `truenas` NFS storage is reported as **active** with 10.37% of its reported capacity in use.

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
- [x] Real cluster/quorum/storage/HA output published

## Real evidence

The sanitized command output is published in **[Cluster Validation](validation.md)** and includes:

```bash
pvecm status
pvecm nodes
pvesm status
ha-manager status
```

The snapshot proves the three-node membership, quorum, active TrueNAS NFS storage, HA CRM state, fencing/watchdog state and the registered HA resource at capture time.
