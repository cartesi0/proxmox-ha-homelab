# Cluster Validation

This page contains **real, sanitized evidence captured from the running homelab**.

<p align="center">
  <img src="../assets/cluster-status.svg" alt="Real Proxmox cluster validation snapshot" width="950">
</p>

## Capture snapshot

- Capture date: **19 August 2026**
- Kernel reported by the node: `7.0.14-12-pve`
- Cluster name: `labcluster`
- Corosync transport: `knet`
- Secure authentication: enabled

## 1. Cluster and quorum

Command:

```bash
pvecm status
```

Real output:

```text
Cluster information
-------------------
Name:             labcluster
Config Version:   3
Transport:        knet
Secure auth:      on

Quorum information
------------------
Date:             Wed Aug 19 17:22:53 2026
Quorum provider:  corosync_votequorum
Nodes:            3
Node ID:          0x00000001
Ring ID:          1.685
Quorate:          Yes

Votequorum information
----------------------
Expected votes:   3
Highest expected: 3
Total votes:      3
Quorum:           2
Flags:            Quorate

Membership information
----------------------
    Nodeid      Votes Name
0x00000001          1 192.168.1.47 (local)
0x00000002          1 192.168.1.48
0x00000003          1 192.168.1.49
```

### Result

The cluster reports **3 nodes, 3 total votes and quorum at 2 votes**. At capture time the cluster was quorate.

## 2. Node membership

Command:

```bash
pvecm nodes
```

Real output:

```text
Membership information
----------------------
    Nodeid      Votes Name
         1          1 proxmox (local)
         2          1 pve2
         3          1 pve3
```

### Result

All three expected Proxmox VE nodes were present in cluster membership.

## 3. Storage status

Command:

```bash
pvesm status
```

Real output:

```text
Name             Type     Status     Total (KiB)      Used (KiB) Available (KiB)        %
local             dir     active        67169672        48329328        15382504   71.95%
local-lvm     lvmthin     active       153374720       109233475        44141244   71.22%
truenas           nfs     active       934724608        96884736       837839872   10.37%
```

### Result

The `truenas` NFS storage was **active** and visible from Proxmox. At capture time it used **10.37%** of the reported capacity.

## 4. HA manager status

Command:

```bash
ha-manager status
```

Real output:

```text
quorum OK
master proxmox (active, Wed Aug 19 17:22:51 2026) - dynamic load CRS (load imbalance: 2.9%)
fencing armed (CRM watchdog active)
lrm proxmox (idle, watchdog standby, Wed Aug 19 17:22:54 2026)
lrm pve2 (active, watchdog active, Wed Aug 19 17:22:55 2026)
lrm pve3 (idle, watchdog standby, Wed Aug 19 17:22:52 2026)
service vm:200 (pve2, stopped)
```

### Result

At capture time:

- HA reported `quorum OK`.
- `proxmox` was the active HA CRM master.
- Fencing was armed and the CRM watchdog was active.
- The HA manager reported `dynamic load CRS` with a **2.9% load imbalance**.
- `pve2` had an active LRM watchdog.
- HA service `vm:200` was registered on `pve2` and was **stopped** at the moment of capture.

The last point is important: this snapshot proves the HA resource exists, but it does **not** claim that VM 200 was running at the time this evidence was collected.

## What this evidence validates

- [x] Three cluster members visible
- [x] Cluster quorum present
- [x] Three expected votes available
- [x] TrueNAS NFS storage active
- [x] HA CRM active
- [x] Fencing armed
- [x] HA watchdog state visible
- [x] HA resource registered
- [x] Dynamic load CRS status visible

## Security note

The output above was reviewed before publication. No passwords, API tokens, private keys, cookies or public credentials are included.

The RFC1918 addresses shown are private lab addresses used to make the topology understandable.
