# Proxmox VE Cluster Setup

## Overview

This homelab uses a three-node Proxmox VE cluster.

The purpose of the cluster is to study and test enterprise virtualization concepts such as:

* Centralized management
* Cluster communication
* Quorum
* Shared storage
* VM migration
* Live migration
* High Availability
* Failure recovery
* Infrastructure troubleshooting

## Cluster Nodes

The current cluster consists of three Proxmox VE hosts.

| Node           | Management IP | Role         |
| -------------- | ------------- | ------------ |
| Proxmox Node 1 | 192.168.1.47  | Cluster node |
| Proxmox Node 2 | 192.168.1.48  | Cluster node |
| Proxmox Node 3 | 192.168.1.49  | Cluster node |

All three nodes participate in the same Proxmox VE cluster.

## Cluster Creation

The first Proxmox VE server was used to create the cluster.

The additional Proxmox VE hosts were then joined to the existing cluster.

After joining the nodes, all systems became visible from the same Proxmox Datacenter interface.

This allows the infrastructure to be managed centrally instead of managing each physical server independently.

## Cluster Communication

Proxmox VE uses Corosync for communication between cluster nodes.

Corosync allows the nodes to exchange information about:

* Cluster membership
* Node availability
* Quorum
* Cluster state
* High Availability operations

Reliable communication between cluster nodes is important for correct cluster operation.

## Quorum

The lab uses three cluster nodes.

With three voting nodes, the cluster normally needs at least two available votes to maintain quorum.

Quorum is important because it prevents different parts of the cluster from independently making conflicting decisions.

A three-node cluster therefore provides a useful environment for testing node failures and observing cluster behavior.

## Cluster Verification

The cluster status can be checked from the Proxmox shell with:

```bash
pvecm status
```

This command displays information such as:

* Cluster name
* Number of nodes
* Quorum status
* Expected votes
* Total votes
* Current cluster membership

The list of cluster nodes can also be checked with:

```bash
pvecm nodes
```

A healthy three-node cluster should show all configured nodes as members of the cluster.

## Corosync Configuration

The Corosync configuration used by Proxmox VE can be inspected with:

```bash
cat /etc/pve/corosync.conf
```

This file contains information about cluster nodes and cluster communication.

Changes to the Corosync configuration should always be performed carefully because incorrect settings can affect the availability of the entire cluster.

## Shared Storage

Shared storage is provided by a TrueNAS server.

The TrueNAS server is currently reachable at:

```text
192.168.10.76
```

Shared storage allows multiple Proxmox VE nodes to access the virtual machine storage.

This is useful for:

* VM migration
* Live migration
* High Availability
* Centralized VM storage

Storage status can be checked from the Proxmox shell with:

```bash
pvesm status
```

## Migration Testing

Virtual machine migration was tested between the Proxmox VE nodes.

A VM was successfully migrated between active nodes.

When the VM storage was already located on shared storage, migration was significantly faster because the entire virtual disk did not need to be copied between physical hosts.

This demonstrated the importance of shared storage in a clustered virtualization environment.

## Live Migration

Live migration was also tested.

During live migration:

1. Both Proxmox VE nodes remain operational.
2. The virtual machine continues running.
3. The VM state is transferred between physical hosts.
4. Service interruption is minimal.

Live migration is useful for maintenance operations and workload relocation without intentionally shutting down the virtual machine.

## High Availability Configuration

A virtual machine was configured as a Proxmox High Availability resource.

The purpose of this test was to verify how the cluster reacts when a physical virtualization host becomes unavailable.

## Physical Node Failure Test

The HA virtual machine was initially running on one Proxmox VE node.

To simulate an unexpected physical server failure, that node was powered off.

The cluster detected that the physical host was no longer available.

The HA manager then restarted the affected virtual machine on another available Proxmox VE node.

The test confirmed that High Availability failover was working correctly.

## High Availability vs Zero Downtime

An important lesson from the test was that High Availability does not necessarily mean zero downtime.

If a physical server suddenly fails, the RAM state of the virtual machines running on that server is lost.

The virtual machine must therefore be started again on another available cluster node.

Proxmox HA provides automatic service recovery, but a short interruption can occur during failure detection and virtual machine restart.

## Live Migration vs HA Failover

### Live Migration

During live migration:

1. Both physical Proxmox hosts are operational.
2. The VM remains active.
3. The VM state is transferred between the hosts.
4. The workload continues running during the migration.

### HA Failover

During an unexpected node failure:

1. The original Proxmox node becomes unavailable.
2. The cluster detects the failure.
3. The HA manager selects another available node.
4. The VM is restarted on the new node.

These two features solve different infrastructure problems.

Live migration is mainly used to move workloads between healthy hosts.

High Availability is used to automatically recover workloads after a physical host failure.

## Useful Diagnostic Commands

### Check cluster status

```bash
pvecm status
```

### List cluster nodes

```bash
pvecm nodes
```

### Check Corosync configuration

```bash
cat /etc/pve/corosync.conf
```

### Check Corosync service

```bash
systemctl status corosync
```

### Check Proxmox cluster service

```bash
systemctl status pve-cluster
```

### Check High Availability services

```bash
systemctl status pve-ha-lrm
systemctl status pve-ha-crm
```

### Check shared storage

```bash
pvesm status
```

### List virtual machines

```bash
qm list
```

### List containers

```bash
pct list
```

## Tests Completed

* [x] Three-node Proxmox VE cluster created
* [x] Additional nodes joined to the cluster
* [x] Cluster communication verified
* [x] Three-node quorum tested
* [x] Shared storage configured
* [x] VM migration tested
* [x] Live migration tested
* [x] HA resource configured
* [x] Physical node failure simulated
* [x] Automatic VM restart on another node verified
* [x] Failed node returned to the cluster

## Lessons Learned

Building and testing this cluster provided practical experience with concepts commonly used in virtualization environments.

The main lessons learned were:

* A cluster provides centralized management of multiple physical hosts.
* Quorum is essential for safe cluster operation.
* Corosync is responsible for communication between Proxmox cluster nodes.
* Shared storage simplifies VM migration between physical hosts.
* Live migration and High Availability solve different problems.
* HA provides automatic workload recovery after host failure.
* HA does not automatically mean zero downtime.
* Failure testing is an important part of validating an HA infrastructure.

## Future Improvements

Future improvements planned for the lab include:

* Resource utilization monitoring
* Migration strategy based on node resource usage
* Dedicated storage network
* Dedicated cluster communication network
* Network segmentation
* VLAN testing
* Firewall isolation
* Backup and restore testing
* Zabbix monitoring and alerting
* Additional HA failure scenarios
* Infrastructure automation
