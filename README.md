# Proxmox VE High Availability Homelab

![Proxmox VE](https://img.shields.io/badge/Proxmox_VE-3--Node_Cluster-E57000?logo=proxmox&logoColor=white)
![High Availability](https://img.shields.io/badge/High_Availability-Tested-success)
![Shared Storage](https://img.shields.io/badge/Shared_Storage-TrueNAS-0095D5?logo=truenas&logoColor=white)
![Validation](https://img.shields.io/badge/Real_Cluster_Evidence-Published-success)

A hands-on three-node Proxmox VE homelab built to study clustering, shared storage, live migration, High Availability (HA), failure recovery and infrastructure troubleshooting.

<p align="center">
  <img src="assets/architecture-overview.svg" alt="Proxmox HA Homelab Architecture" width="900">
</p>

## Why this project exists

The goal is to move beyond theory and document a working virtualization lab with repeatable tests and real failure scenarios. The repository is intentionally focused on what has actually been configured and tested.

## Current lab

| Component | Address | Purpose |
|---|---:|---|
| Proxmox VE Node 1 | `192.168.1.47` | Cluster node |
| Proxmox VE Node 2 | `192.168.1.48` | Cluster node |
| Proxmox VE Node 3 | `192.168.1.49` | Cluster node |
| TrueNAS | `192.168.10.76` | Shared storage |

## Real cluster snapshot

The following status card is generated from sanitized command output collected from the running lab on **19 August 2026**.

<p align="center">
  <img src="assets/cluster-status.svg" alt="Real cluster validation snapshot" width="950">
</p>

Full evidence: **[Cluster Validation](docs/validation.md)**

## What has been tested

- [x] Three-node Proxmox VE cluster
- [x] Shared storage available to the cluster
- [x] Manual VM migration
- [x] Live migration
- [x] HA resource configuration
- [x] Physical node failure simulation
- [x] Automatic VM restart on another node
- [x] Cluster recovery after the failed node returned
- [x] Sanitized command-output validation evidence
- [ ] Dedicated storage / cluster network testing
- [ ] Monitoring and alerting
- [ ] Backup and restore validation
- [ ] Infrastructure automation

## Key lesson: Live Migration is not HA Failover

**Live migration** moves a running VM between healthy hosts with minimal interruption.

**HA failover** reacts to an unexpected host failure. Because the failed host's RAM state is gone, the workload must be restarted on another available node. HA provides automatic recovery, not zero downtime.

<p align="center">
  <img src="assets/ha-failover.svg" alt="Live Migration versus HA Failover" width="900">
</p>

## Documentation

| Document | What it covers |
|---|---|
| [Architecture](docs/architecture.md) | Components, topology and design choices |
| [Cluster Setup](docs/cluster-setup.md) | Three-node cluster, Corosync and quorum |
| [Shared Storage](docs/shared-storage.md) | Why shared storage matters for migration and HA |
| [High Availability](docs/high-availability.md) | Failure test, failover behavior and limitations |
| [Troubleshooting](docs/troubleshooting.md) | Problems observed and how they were interpreted |
| [Validation](docs/validation.md) | Real sanitized output proving cluster, storage and HA state |
| [Config Examples](configs/examples/README.md) | Safe examples and rules for publishing configuration |

## Validation highlights

Real command output currently confirms:

- Cluster name `labcluster`
- 3 nodes / 3 votes / quorum present
- TrueNAS NFS storage active
- HA CRM active
- Fencing armed and watchdog state visible
- HA resource `vm:200` registered on `pve2`
- Dynamic load CRS reported with 2.9% load imbalance at capture time

Validation commands used:

```bash
pvecm status
pvecm nodes
pvesm status
ha-manager status
```

## Skills demonstrated

- Proxmox VE administration
- Linux systems administration
- Cluster concepts and quorum
- Corosync awareness
- Shared storage concepts
- VM migration and live migration
- High Availability testing
- Failure analysis and troubleshooting
- Validation from real command output
- Technical documentation with Markdown and diagrams

## Roadmap

Planned improvements include dedicated network segmentation, monitoring with Zabbix, backup/recovery testing, additional HA scenarios and infrastructure automation.

---

> This is a personal learning homelab. The documentation reflects hands-on tests performed in the environment and will evolve as new scenarios are validated.
