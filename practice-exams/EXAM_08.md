# CKA Practice Exam 8 - Cluster Maintenance & Operations

**Duration**: 2 hours  
**Total Tasks**: 16  
**Passing Score**: 66% (74% for CKA)

---

## Task 1: Node Cordon (6 minutes)

Cordon node `worker-1` to prevent new pods from scheduling.

---

## Task 2: Node Drain (10 minutes)

Drain node `worker-1`:
- Ignore DaemonSets
- Delete emptyDir data
- Verify pods rescheduled
- Document procedure

---

## Task 3: Node Uncordon (6 minutes)

Uncordon node `worker-1` and verify it's schedulable.

---

## Task 4: Node Labeling (8 minutes)

Label node `worker-1` with:
- `zone=east`
- `node-type=compute`
- `environment=production`
- Verify labels

---

## Task 5: Node Taints (10 minutes)

Taint node `worker-1` with:
- Key: `dedicated`
- Value: `special`
- Effect: `NoSchedule`
- Verify taint applied

---

## Task 6: Pod with Tolerations (10 minutes)

Create pod with toleration that:
- Matches the taint from Task 5
- Can schedule on tainted node
- Verify scheduling

---

## Task 7: etcd Backup (12 minutes)

Document etcd backup procedure:
1. Backup command
2. Verify backup
3. Backup location
4. Backup verification steps

---

## Task 8: Resource Export (10 minutes)

Export all resources from namespace `production`:
1. Export to YAML
2. Verify export
3. Document restore procedure

---

## Task 9: Cluster Version Check (8 minutes)

Check Kubernetes cluster version:
1. API server version
2. kubectl version
3. Node kubelet versions
4. Document version differences

---

## Task 10: Node Conditions (10 minutes)

Check node conditions:
1. List all nodes
2. Describe node conditions
3. Identify any issues
4. Document findings

---

## Task 11: Resource Usage (10 minutes)

Check cluster resource usage:
1. Node capacity
2. Node allocatable
3. Pod resource usage (if metrics-server available)
4. Identify resource pressure

---

## Task 12: Pod Disruption Budget (12 minutes)

Create PDB that:
- Applies to deployment `critical-app`
- Ensures minimum 3 pods available
- Test by draining node
- Verify PDB is respected

---

## Task 13: ResourceQuota Management (12 minutes)

1. Create ResourceQuota in namespace
2. Check quota usage
3. Identify what's consuming quota
4. Optimize or increase quota

---

## Task 14: LimitRange Management (10 minutes)

1. Create LimitRange in namespace
2. Create pod without resources
3. Verify defaults applied
4. Test min/max constraints

---

## Task 15: Cluster Health Check (12 minutes)

Perform complete cluster health check:
1. Node status
2. System pods status
3. API server health
4. etcd health (if accessible)
5. Document any issues

---

## Task 16: Maintenance Window Procedure (12 minutes)

Document complete maintenance procedure:
1. Pre-maintenance checks
2. Backup procedures
3. Maintenance steps
4. Post-maintenance verification
5. Rollback plan

---

## Scoring

- **Tasks 1-6**: 6 points each (36 points)
- **Tasks 7-10**: 6 points each (24 points)
- **Tasks 11-14**: 6 points each (24 points)
- **Tasks 15-16**: 8 points each (16 points)

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

**Good luck!** 🚀

