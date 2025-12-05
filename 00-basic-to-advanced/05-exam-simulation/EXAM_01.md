# CKA Practice Exam 1

**Time Limit**: 2 hours  
**Total Questions**: 17  
**Passing Score**: 66% (74% for actual CKA)

---

## Instructions

1. You have access to a Kubernetes cluster
2. You can use `kubectl`, `kubectl explain`, and Kubernetes documentation
3. Work efficiently - time management is critical
4. Verify your work after completing each task
5. All tasks should be completed in the `default` namespace unless specified

---

## Question 1: Pod Troubleshooting (8 minutes)

**Scenario**: A pod named `web-app` in the `production` namespace is in `CrashLoopBackOff` state.

**Tasks**:
1. Identify why the pod is crashing
2. Fix the issue
3. Verify the pod is running

**Hints**:
- Check pod logs
- Check pod events
- Verify image and command

---

## Question 2: Create Deployment (5 minutes)

**Task**: Create a deployment named `api-server` with the following requirements:
- Image: `nginx:1.21`
- Replicas: 3
- Labels: `app=api`, `tier=backend`
- Resource requests: CPU 100m, Memory 128Mi
- Resource limits: CPU 200m, Memory 256Mi

---

## Question 3: Service Configuration (6 minutes)

**Scenario**: A service named `api-service` exists but has no endpoints.

**Tasks**:
1. Identify why there are no endpoints
2. Fix the service selector to match existing pods
3. Verify endpoints are created

**Hints**:
- Check service selector
- Check pod labels
- Ensure labels match

---

## Question 4: ConfigMap and Pod (7 minutes)

**Task**: 
1. Create a ConfigMap named `app-config` with:
   - `database_url=postgresql://db:5432`
   - `log_level=info`
2. Create a pod named `config-pod` using image `busybox:1.35` that:
   - Mounts the ConfigMap as a volume at `/etc/config`
   - Runs command: `sh -c 'cat /etc/config/* && sleep 3600'`

---

## Question 5: Secret Management (6 minutes)

**Task**:
1. Create a secret named `db-credentials` with:
   - `username=admin`
   - `password=secret123`
2. Create a pod named `db-client` that uses these secrets as environment variables:
   - `DB_USER` from secret key `username`
   - `DB_PASS` from secret key `password`

---

## Question 6: Deployment Update (8 minutes)

**Scenario**: A deployment `frontend` is running version `nginx:1.20`. You need to update it to `nginx:1.22`.

**Tasks**:
1. Update the deployment image
2. Monitor the rollout
3. If the new version has issues, rollback to the previous version
4. Verify the deployment is healthy

---

## Question 7: PersistentVolume and PVC (10 minutes)

**Task**:
1. Create a PersistentVolume named `pv-manual` with:
   - Storage: 5Gi
   - AccessMode: ReadWriteOnce
   - StorageClass: `manual`
   - HostPath: `/mnt/data`
2. Create a PersistentVolumeClaim named `data-pvc` that binds to the PV
3. Create a pod named `storage-pod` that uses the PVC mounted at `/data`

---

## Question 8: Resource Quota (7 minutes)

**Task**: Create a ResourceQuota in namespace `dev` with:
- CPU requests: 2 cores
- Memory requests: 4Gi
- CPU limits: 4 cores
- Memory limits: 8Gi
- Maximum pods: 10

---

## Question 9: RBAC Configuration (10 minutes)

**Task**: 
1. Create a ServiceAccount named `app-sa` in namespace `production`
2. Create a Role named `pod-reader` that allows:
   - `get`, `list`, `watch` on pods
3. Create a RoleBinding that binds the ServiceAccount to the Role
4. Verify the permissions work

---

## Question 10: NetworkPolicy (12 minutes)

**Task**: Create NetworkPolicies for a 3-tier application:
1. **Frontend pods** (label: `app=frontend`):
   - Allow ingress on port 80 from anywhere
   - Allow egress to backend pods on port 8080
2. **Backend pods** (label: `app=backend`):
   - Allow ingress from frontend pods on port 8080
   - Allow egress to database pods on port 3306
3. **Database pods** (label: `app=database`):
   - Allow ingress from backend pods on port 3306
   - Deny all other traffic

---

## Question 11: Ingress Configuration (8 minutes)

**Task**: Create an Ingress resource named `web-ingress` that:
- Routes traffic from `app.example.com` to service `web-service` on port 80
- Uses ingress class `nginx`
- Has a path `/api` that routes to service `api-service` on port 8080

---

## Question 12: StatefulSet (12 minutes)

**Task**: Create a StatefulSet named `db-statefulset` with:
- Service: Headless service named `db-headless`
- Replicas: 3
- Image: `mysql:8.0`
- Environment variables:
  - `MYSQL_ROOT_PASSWORD=rootpass`
  - `MYSQL_DATABASE=mydb`
- Persistent storage: 10Gi per pod using StorageClass `fast-ssd`

---

## Question 13: Job and CronJob (10 minutes)

**Task**:
1. Create a Job named `data-processor` that:
   - Uses image `busybox:1.35`
   - Runs command: `echo "Processing data" && sleep 30`
   - Has a deadline of 5 minutes
2. Create a CronJob named `backup-job` that:
   - Runs daily at 2 AM
   - Uses same image and command as the Job
   - Keeps 3 successful job histories

---

## Question 14: DaemonSet (7 minutes)

**Task**: Create a DaemonSet named `log-collector` that:
- Runs on all nodes
- Uses image `busybox:1.35`
- Has tolerations for master/control-plane nodes
- Command: `sh -c 'echo "Collecting logs" && sleep 3600'`

---

## Question 15: Node Maintenance (8 minutes)

**Task**: Perform maintenance on node `worker-node-1`:
1. Cordon the node
2. Drain the node (ignore DaemonSets)
3. Simulate maintenance (just wait 30 seconds)
4. Uncordon the node
5. Verify pods rescheduled

---

## Question 16: Pod Disruption Budget (6 minutes)

**Task**: Create a PodDisruptionBudget for deployment `web-app` (5 replicas) that:
- Ensures minimum 3 pods are available
- Applies to pods with label `app=web`

---

## Question 17: Troubleshooting Multi-Issue (15 minutes)

**Scenario**: Multiple issues in the cluster:
1. Pod `app-pod` is in `Pending` state
2. Service `api-svc` has no endpoints
3. Deployment `frontend` rollout is stuck

**Tasks**:
1. Diagnose all issues
2. Fix each issue
3. Verify all are resolved

---

## Scoring Guide

- **Question 1**: 6 points
- **Question 2**: 4 points
- **Question 3**: 5 points
- **Question 4**: 6 points
- **Question 5**: 5 points
- **Question 6**: 7 points
- **Question 7**: 8 points
- **Question 8**: 6 points
- **Question 9**: 8 points
- **Question 10**: 10 points
- **Question 11**: 7 points
- **Question 12**: 10 points
- **Question 13**: 8 points
- **Question 14**: 6 points
- **Question 15**: 7 points
- **Question 16**: 5 points
- **Question 17**: 12 points

**Total**: 120 points  
**Passing**: 79 points (66%) | 89 points (74% for CKA)

---

## Time Management Tips

- Allocate ~7 minutes per question on average
- Skip difficult questions and return later
- Verify work as you go
- Use `kubectl explain` when unsure
- Check documentation for syntax

---

## Solutions

Solutions will be provided after you complete the exam. Focus on understanding the concepts rather than just copying solutions.

Good luck! 🚀

