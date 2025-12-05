# CKA Practice Exam 3

**Duration**: 2 hours  
**Total Tasks**: 16  
**Passing Score**: 66% (74% for CKA)

---

## Task 1: Complete Application Deployment (20 minutes)

Deploy a complete 3-tier application:

1. **Database Layer**:
   - StatefulSet `db-statefulset` with 3 replicas
   - Image: `mysql:8.0` with required env vars
   - Headless service `db-headless`
   - Persistent storage: 10Gi per pod

2. **Backend Layer**:
   - Deployment `backend-deployment` with 5 replicas
   - Image: `nginx:1.21`
   - Service `backend-service` (ClusterIP) on port 8080
   - ConfigMap `backend-config` with `DB_HOST=db-headless`

3. **Frontend Layer**:
   - Deployment `frontend-deployment` with 3 replicas
   - Image: `nginx:1.21`
   - Service `frontend-service` (NodePort) on port 30080
   - Ingress `app-ingress` routing to frontend-service

---

## Task 2: Pod with Node Affinity (12 minutes)

Create a pod `affinity-pod` that:
- Has required node affinity: zone in [us-east-1a, us-east-1b]
- Has preferred node affinity: node-type=compute (weight 100)
- Uses image `nginx:1.21`

First, label nodes appropriately.

---

## Task 3: Pod with Pod Affinity (12 minutes)

Create deployment `cache-deployment` (2 replicas) and `app-deployment` (3 replicas):
- `app-deployment` pods should prefer to be co-located with `cache-deployment` pods
- Use pod affinity with topology key `kubernetes.io/hostname`

---

## Task 4: Advanced RBAC (15 minutes)

1. Create Role `resource-manager` that allows:
   - All verbs on ConfigMaps and Secrets
   - `get`, `list`, `watch` on pods
   - `create`, `update` on deployments

2. Create RoleBinding for ServiceAccount `manager-sa` in namespace `production`

3. Create ClusterRole `cluster-admin-helper` that aggregates to `cluster-admin` role

---

## Task 5: NetworkPolicy Default Deny (12 minutes)

In namespace `secure`:
1. Create default deny-all NetworkPolicy
2. Allow pods with label `app=web` to receive traffic on port 80
3. Allow pods with label `app=web` to send traffic to pods with label `app=api` on port 8080
4. Deny all other traffic

---

## Task 6: Ingress with Multiple Hosts (10 minutes)

Create Ingress `multi-host-ingress` that:
- Routes `api.example.com` to `api-service:80`
- Routes `web.example.com` to `web-service:80`
- Routes `admin.example.com` to `admin-service:80`
- Uses ingress class `nginx`

---

## Task 7: StatefulSet Scaling (10 minutes)

1. Scale StatefulSet `db-statefulset` from 3 to 5 replicas
2. Verify all pods are running
3. Check that each pod has its own PVC
4. Scale back to 3 replicas

---

## Task 8: Job with TTL (10 minutes)

Create Job `cleanup-job` that:
- Uses image `busybox:1.35`
- Command: `echo "Cleanup done" && date`
- Has TTL of 100 seconds (auto-delete after completion)
- Verify it gets deleted automatically

---

## Task 9: CronJob Suspend/Resume (8 minutes)

1. Create CronJob `backup-cronjob` that runs every 5 minutes
2. Suspend it
3. Verify it's suspended
4. Resume it
5. Manually trigger a job from the CronJob

---

## Task 10: ResourceQuota with Scopes (12 minutes)

Create ResourceQuota `team-quota` in namespace `team-a` that:
- Limits CPU requests to 5 cores
- Limits memory requests to 10Gi
- Limits pods to 20
- Scope: `NotTerminating` (only applies to non-terminating pods)

---

## Task 11: Pod Security Standards (12 minutes)

1. Create namespace `restricted-ns` with Pod Security Standard `restricted`

2. Create a pod `secure-pod` in this namespace that:
   - Runs as non-root (UID 1000)
   - Has read-only root filesystem
   - Drops all capabilities
   - Uses seccomp profile `RuntimeDefault`

---

## Task 12: Troubleshooting Multiple Issues (15 minutes)

Fix the following issues in namespace `production`:

1. Pod `app-pod` is in `Pending` state
2. Service `app-service` has no endpoints
3. Deployment `app-deployment` rollout is stuck
4. PVC `data-pvc` is not binding

Diagnose and fix each issue.

---

## Task 13: etcd Backup (10 minutes)

Document the procedure to:
1. Backup etcd (assume control plane node access)
2. Verify the backup
3. List the steps for restore (you don't need to actually restore)

---

## Task 14: Node Drain with PDB (12 minutes)

1. Create deployment `critical-app` with 5 replicas
2. Create PDB ensuring minimum 3 pods available
3. Drain a node that has pods from this deployment
4. Verify PDB is respected (only 1 pod can be evicted at a time)

---

## Task 15: ConfigMap Immutable (8 minutes)

1. Create ConfigMap `immutable-config` with key `version=v1`
2. Mark it as immutable
3. Try to update it (should fail)
4. Create a new ConfigMap `immutable-config-v2` with updated values

---

## Task 16: Service Headless (10 minutes)

1. Create headless service `db-headless` (ClusterIP: None)
2. Create 3 pods with label `app=database`
3. Test DNS resolution from another pod
4. Verify all pod IPs are returned

---

## Scoring

- **Task 1**: 12 points
- **Task 2**: 7 points
- **Task 3**: 7 points
- **Task 4**: 9 points
- **Task 5**: 7 points
- **Task 6**: 6 points
- **Task 7**: 6 points
- **Task 8**: 6 points
- **Task 9**: 5 points
- **Task 10**: 7 points
- **Task 11**: 7 points
- **Task 12**: 9 points
- **Task 13**: 6 points
- **Task 14**: 7 points
- **Task 15**: 5 points
- **Task 16**: 6 points

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

**Good luck!** 🚀

