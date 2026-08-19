# Cluster Validation

This page contains **real evidence captured from the running homelab and sanitized before publication**.

<p align="center">
  <img src="../assets/cluster-status.svg" alt="Sanitized Proxmox cluster validation snapshot" width="950">
</p>

## Privacy note

Operational identifiers that are not needed to demonstrate the technical result have been removed or generalized. Values inside angle brackets are deliberate placeholders.

## Capture snapshot

- Capture date: **19 August 2026**
- Platform: Proxmox VE / Linux
- Corosync transport: `knet`
- Secure authentication: enabled

## 1. Cluster and quorum

Command:

```bash
pvecm status
```

Sanitized excerpt from the real output:

```text
Cluster information
-------------------
Name:             <LAB_CLUSTER>
Config Version:   3
Transport:        knet
Secure auth:      on

Quorum information
------------------
Nodes:            3
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
Node A             1 vote
Node B             1 vote
Node C             1 vote
```

### Result

The cluster reports **3 nodes, 3 total votes and quorum at 2 votes**. At capture time the cluster was quorate.

## 2. Node membership

Command:

```bash
pvecm nodes
```

Sanitized excerpt:

```text
Membership information
----------------------
Node A    1 vote
Node B    1 vote
Node C    1 vote
```

### Result

All three expected Proxmox VE nodes were present in cluster membership.

## 3. Storage status

Command:

```bash
pvesm status
```

Sanitized excerpt:

```text
Name            Type      Status
local           dir       active
local-lvm       lvmthin   active
truenas         nfs       active
```

### Result

The TrueNAS NFS storage was **active** and visible from Proxmox at capture time.

## 4. HA manager status

Command:

```bash
ha-manager status
```

Sanitized excerpt:

```text
quorum OK
master <NODE_A> (active) - dynamic load CRS (load imbalance: 2.9%)
fencing armed (CRM watchdog active)
lrm <NODE_A> (...)
lrm <NODE_B> (...)
lrm <NODE_C> (...)
service <HA_VM> (<NODE_B>, stopped)
```

### Result

At capture time:

- HA reported `quorum OK`.
- An HA CRM master was active.
- Fencing was armed and the CRM watchdog was active.
- The HA manager reported `dynamic load CRS` with a **2.9% load imbalance**.
- The HA resource was registered and was **stopped** at the moment of capture.

This snapshot proves the HA resource exists, but it does **not** claim that the HA VM was running at the time this evidence was collected.

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

## Publication rule

Only technical details needed to explain the lab result are kept in the public version. Network and account-specific identifiers are generalized.
