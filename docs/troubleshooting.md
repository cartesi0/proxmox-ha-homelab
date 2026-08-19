# Troubleshooting Notes

This page records problems observed during the lab and the reasoning used to understand them. The goal is to document investigation, not just successful configuration.

## Migration unexpectedly slow

### Symptom

A VM migration started transferring data for much longer than expected.

### Cause

The migration involved local disk data, so Proxmox had to copy the virtual disk between hosts.

### What changed

When the VM used shared storage, migration became much faster because the disk was already accessible from both nodes.

### Lesson

A Proxmox cluster does not automatically imply shared storage. Storage placement directly affects migration behavior.

---

## HA failover restarted the VM

### Symptom

After powering off the node hosting the HA VM, the workload appeared on another node but the VM restarted.

### Interpretation

This is expected HA behavior after a sudden physical host failure. The failed node's RAM state is no longer available, so another node can restart the workload from its disk but cannot continue the lost in-memory execution state.

### Lesson

**HA failover and live migration are different mechanisms.** HA is automatic recovery; live migration is a planned transfer between healthy hosts.

---

## VM did not automatically return to the original node

### Symptom

After the failed node returned to the cluster, the recovered VM remained on the node where HA had restarted it.

### Interpretation

Rejoining the cluster does not mean the workload must immediately move back. Migration can be performed later, and placement policies should be designed intentionally rather than assuming automatic failback.

---

## Useful diagnostic commands

```bash
pvecm status
pvecm nodes
pvesm status
ha-manager status
systemctl status corosync
systemctl status pve-cluster
systemctl status pve-ha-lrm
systemctl status pve-ha-crm
```

## Documentation rule

When adding a troubleshooting case, record:

1. Symptom
2. Expected behavior
3. Evidence collected
4. Root cause or best-supported explanation
5. Fix or next test
6. Lesson learned

Avoid publishing secrets, private keys, tokens or credentials in command output.
