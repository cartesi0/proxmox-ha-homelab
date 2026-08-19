# High Availability Testing

## Goal

The HA test was designed to answer a practical question: what happens to a VM when the physical Proxmox host running it suddenly becomes unavailable?

<p align="center">
  <img src="../assets/ha-failover.svg" alt="HA failover behavior" width="900">
</p>

## Test scenario

1. A VM was configured as a Proxmox HA resource.
2. The VM was running on one cluster node.
3. That physical node was powered off to simulate an unexpected host failure.
4. The cluster detected the loss of the node.
5. Proxmox HA started the VM on another available node.

## Result

The HA test succeeded: the workload recovered automatically on another node.

The VM did **not** continue from the exact RAM state of the failed host. Because that state was lost with the physical server, the VM restarted on the surviving node.

## HA is not zero downtime

This distinction matters in production infrastructure:

- **Live migration** is a planned move between healthy hosts.
- **HA failover** is an automatic recovery action after a host failure.

HA reduces recovery effort and service outage, but it does not guarantee uninterrupted execution after a sudden physical failure.

## Real HA runtime snapshot

A later validation capture from the running lab returned:

```text
quorum OK
master proxmox (active, Wed Aug 19 17:22:51 2026) - dynamic load CRS (load imbalance: 2.9%)
fencing armed (CRM watchdog active)
lrm proxmox (idle, watchdog standby, Wed Aug 19 17:22:54 2026)
lrm pve2 (active, watchdog active, Wed Aug 19 17:22:55 2026)
lrm pve3 (idle, watchdog standby, Wed Aug 19 17:22:52 2026)
service vm:200 (pve2, stopped)
```

This capture shows that, at that moment:

- Quorum was healthy.
- `proxmox` was the active CRM master.
- Fencing was armed.
- CRM watchdog was active.
- `pve2` had the active LRM watchdog.
- The scheduler reported `dynamic load CRS` with a 2.9% load imbalance.
- HA service `vm:200` was registered on `pve2`, but the VM itself was **stopped** at capture time.

<p align="center">
  <img src="../assets/cluster-status.svg" alt="Real HA and cluster runtime status" width="950">
</p>

The complete sanitized evidence is available in **[Cluster Validation](validation.md)**.

## What happens when the failed node returns?

When the failed Proxmox host comes back online, it rejoins the cluster. The VM that was recovered on another node does not automatically need to move back to its original host. Workload placement can be changed later through migration or HA policy decisions.

## Validation commands

```bash
ha-manager status
pvecm status
pvecm nodes
```

## Future tests

- Failure of a different node
- Multiple HA resources
- Service recovery timing
- Behavior during storage unavailability
- Backup and restore recovery
- Resource utilization monitoring and scheduler behavior with multiple running workloads
