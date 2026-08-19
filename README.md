# Proxmox VE High Availability Homelab

A three-node Proxmox VE homelab built to study and test virtualization,
shared storage, live migration and high availability.

## Project Goals

The goal of this project is to gain hands-on experience with enterprise
virtualization concepts using Proxmox VE.

The lab focuses on:

- Proxmox VE clustering
- High Availability (HA)
- Shared storage
- Live migration
- VM failover
- Infrastructure troubleshooting
- Linux and networking administration

## Lab Architecture

The environment consists of three Proxmox VE nodes:

| Node | Management IP |
|------|---------------|
| Proxmox Node 1 | 192.168.1.47 |
| Proxmox Node 2 | 192.168.1.48 |
| Proxmox Node 3 | 192.168.1.49 |

Shared storage is provided by a TrueNAS server.

| System | IP |
|--------|----|
| TrueNAS | 192.168.10.76 |

## High Availability Test

A virtual machine was configured as a Proxmox HA resource.

The VM was initially running on one cluster node.

To simulate a physical host failure, the node was powered off.

The Proxmox cluster detected the unavailable node and automatically
restarted the VM on another available node.

This test demonstrated an important difference between:

### Live Migration

The VM is moved between active Proxmox nodes while it continues running.

### HA Failover

If a physical node suddenly fails, the VM cannot continue executing from
the RAM of the failed server.

Proxmox HA detects the failure and starts the VM on another available node.

This means HA provides service recovery, but it does not mean zero downtime.

## Tests Completed

- [x] Three-node Proxmox cluster
- [x] Shared storage configuration
- [x] Manual VM migration
- [x] Live migration
- [x] HA resource configuration
- [x] Physical node failure simulation
- [x] Automatic VM restart on another node
- [ ] Resource balancing tests
- [ ] Network segmentation
- [ ] Monitoring and alerting
- [ ] Infrastructure automation

## Documentation

Detailed documentation will be added to the `docs` directory.

Topics will include:

- Cluster configuration
- Storage configuration
- High Availability
- Live migration
- Failure testing
- Troubleshooting

## What I Learned

This lab helped me understand the practical difference between VM migration
and High Availability.

It also provided hands-on experience with cluster management, shared
storage, failure scenarios and infrastructure troubleshooting.

## Future Improvements

Planned improvements include:

- Resource balancing between nodes
- Network segmentation
- Monitoring with Zabbix
- Infrastructure automation
- Backup and recovery testing
- Additional failure scenarios
