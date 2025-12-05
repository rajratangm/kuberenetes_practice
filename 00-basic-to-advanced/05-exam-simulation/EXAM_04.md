# CKA Practice Exam 4

**Time Limit**: 2 hours  
**Total Questions**: 19  
**Passing Score**: 66% (74% for actual CKA)

---

## Instructions

- Complete all tasks in order
- Verify each solution before proceeding
- Use `kubectl explain` when needed
- Time management is essential

---

## Question 1: Pod with Multiple Containers (8 minutes)

**Task**: Create pod `multi-pod` with:
- Init container: Download config from URL (simulate with `sleep 5`)
- Main container: `nginx:1.21` on port 80
- Sidecar container: `busybox:1.35` that tails logs from shared volume
- Shared volume: `emptyDir` mounted in both containers

---

## Question 2: Deployment Scaling Issue (7 minutes)

**Scenario**: Deployment `app-deploy` won't scale beyond 3 replicas despite requesting 5.

**Tasks**:
1. Identify why scaling is limited
2. Check ResourceQuota
3. Fix the issue
4. Scale to 5 replicas successfully

---

## Question 3: Service Session Affinity (6 minutes)

**Task**: 
1. Create deployment `sticky-app` with 3 replicas
2. Create service `sticky-svc` with:
   - Session affinity: `ClientIP`
   - Timeout: 3 hours
3. Test session affinity works

---

## Question 4: ConfigMap Hot Reload (8 minutes)

**Task**:
1. Create ConfigMap `hot-config` with `key=value1`
2. Create pod `config-pod` mounting ConfigMap as volume
3. Update ConfigMap to `key=value2`
4. Check if pod sees the change (may need restart)
5. Restart pod to get new config

---

## Question 5: Secret Decoding (5 minutes)

**Task**:
1. Create secret `encoded-secret` with `password=secret123`
2. Retrieve and decode the secret value
3. Verify it matches original

---

## Question 6: Deployment Rollback History (7 minutes)

**Task**:
1. Create deployment `versioned-app` with `nginx:1.20`
2. Update to `nginx:1.21` (revision 2)
3. Update to `nginx:1.22` (revision 3)
4. View rollout history with change causes
5. Rollback to revision 1

---

## Question 7: StorageClass as Default (7 minutes)

**Task**:
1. Create StorageClass `standard-ssd`
2. Set it as default StorageClass
3. Create PVC `default-pvc` without specifying storageClassName
4. Verify it uses the default StorageClass

---

## Question 8: LimitRange for PVCs (6 minutes)

**Task**: Create LimitRange in namespace `storage`:
- Default PVC size: 10Gi
- Maximum PVC size: 50Gi
- Minimum PVC size: 1Gi
- Create PVC within these limits

---

## Question 9: ClusterRole Aggregation (9 minutes)

**Task**:
1. Create ClusterRole `view-pods` with label for aggregation
2. Create ClusterRole `view-services` with same aggregation label
3. Create aggregated ClusterRole `viewer` that combines both
4. Verify aggregated permissions

---

## Question 10: NetworkPolicy Default Deny (10 minutes)

**Task**: In namespace `isolated`:
1. Create default deny-all NetworkPolicy
2. Create NetworkPolicy allowing:
   - Pods with `app=web` to receive traffic on port 80
   - Pods with `app=api` to receive from `app=web` on port 8080
3. Test connectivity

---

## Question 11: Ingress Default Backend (8 minutes)

**Task**: Create Ingress `default-ingress`:
- Default backend: `default-service:80`
- Rules:
  - `app.example.com/api` → `api-service:8080`
  - `app.example.com/web` → `web-service:80`
- All other traffic goes to default backend

---

## Question 12: StatefulSet Ordered Scaling (9 minutes)

**Task**:
1. Create StatefulSet `ordered-app` with 5 replicas
2. Scale down to 2 replicas (verify order: 4, 3 deleted)
3. Scale up to 5 replicas (verify order: 3, 4 created)
4. Verify pod names follow pattern

---

## Question 13: Job with Indexed Completion (8 minutes)

**Task**: Create Job `indexed-job`:
- Completions: 5
- Parallelism: 2
- Completion mode: `Indexed`
- Command uses `$JOB_COMPLETION_INDEX`
- Verify each pod gets unique index

---

## Question 14: CronJob Suspend/Resume (6 minutes)

**Task**:
1. Create CronJob `scheduled-task`
2. Suspend it
3. Verify no jobs are created
4. Resume it
5. Manually trigger a job
6. Verify job runs

---

## Question 15: DaemonSet Update Strategy (7 minutes)

**Task**:
1. Create DaemonSet `updatable-ds`
2. Configure `RollingUpdate` strategy with `maxUnavailable: 1`
3. Update image
4. Monitor rolling update
5. Verify all nodes updated

---

## Question 16: Node Labels and Selectors (8 minutes)

**Task**:
1. Label node `worker-1` with `zone=east`, `node-type=compute`
2. Create pod `zoned-pod` that:
   - Must run on nodes with `zone=east`
   - Prefers nodes with `node-type=compute`
3. Verify pod schedules correctly

---

## Question 17: Pod Anti-Affinity (9 minutes)

**Task**:
1. Create deployment `distributed-app` with 5 replicas
2. Configure pod anti-affinity:
   - Required: No two pods on same node
   - Topology key: `kubernetes.io/hostname`
3. Verify pods distributed across nodes

---

## Question 18: Resource Monitoring (7 minutes)

**Task**:
1. Check resource usage of all pods
2. Identify top 3 CPU-consuming pods
3. Identify top 3 memory-consuming pods
4. Check node resource allocation
5. Find pods without resource limits

---

## Question 19: Complete Troubleshooting (18 minutes)

**Scenario**: Production outage - multiple issues:

1. **Deployment Issue**: `web-deploy` stuck in rollout
2. **Service Issue**: `api-svc` has no endpoints
3. **Storage Issue**: PVC `data-pvc` in `Pending` state
4. **Network Issue**: Pods can't reach external services
5. **RBAC Issue**: ServiceAccount can't list pods

**Tasks**:
1. Diagnose each issue
2. Fix all problems
3. Verify application works end-to-end
4. Document root causes

---

## Scoring Guide

- **Question 1**: 7 points
- **Question 2**: 6 points
- **Question 3**: 5 points
- **Question 4**: 7 points
- **Question 5**: 4 points
- **Question 6**: 6 points
- **Question 7**: 6 points
- **Question 8**: 5 points
- **Question 9**: 8 points
- **Question 10**: 9 points
- **Question 11**: 7 points
- **Question 12**: 8 points
- **Question 13**: 7 points
- **Question 14**: 5 points
- **Question 15**: 6 points
- **Question 16**: 7 points
- **Question 17**: 8 points
- **Question 18**: 6 points
- **Question 19**: 16 points

**Total**: 126 points  
**Passing**: 83 points (66%) | 93 points (74% for CKA)

---

## Time Management

- Average: ~6 minutes per question
- Complex questions: 8-10 minutes
- Simple questions: 4-6 minutes
- Final troubleshooting: 18 minutes
- Review: 10 minutes

Stay focused and verify your work! 🚀

