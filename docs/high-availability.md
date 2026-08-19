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
- Resource utilization monitoring before migration decisions
