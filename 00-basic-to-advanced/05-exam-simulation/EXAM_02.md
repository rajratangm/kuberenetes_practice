# CKA Practice Exam 2

**Time Limit**: 2 hours  
**Total Questions**: 18  
**Passing Score**: 66% (74% for actual CKA)

---

## Instructions

1. Work in the provided Kubernetes cluster
2. Use `kubectl` and available documentation
3. Manage your time carefully
4. Verify solutions before moving on
5. Default namespace unless specified otherwise

---

## Question 1: Pod Crash Investigation (10 minutes)

**Scenario**: Pod `api-pod` in namespace `production` keeps crashing.

**Tasks**:
1. Investigate the crash logs
2. Check previous container logs
3. Identify the root cause
4. Fix the issue
5. Verify pod is running

---

## Question 2: Deployment with Health Probes (8 minutes)

**Task**: Create a deployment `web-deploy` with:
- Image: `nginx:1.21`
- Replicas: 4
- Readiness probe: HTTP GET on `/` port 80, delay 5s, period 10s
- Liveness probe: HTTP GET on `/` port 80, delay 15s, period 20s
- Resource limits: CPU 200m, Memory 256Mi

---

## Question 3: Service Type Configuration (7 minutes)

**Task**: 
1. Create a deployment `app-deploy` with 2 replicas
2. Expose it as:
   - ClusterIP service `app-internal` on port 80
   - NodePort service `app-external` on port 80 (node port 30080)
3. Verify both services work

---

## Question 4: ConfigMap with Multiple Keys (8 minutes)

**Task**:
1. Create ConfigMap `multi-config` from file:
   ```yaml
   database:
     host: db.example.com
     port: "5432"
   redis:
     host: redis.example.com
     port: "6379"
   ```
2. Create pod `config-reader` that:
   - Mounts ConfigMap at `/etc/config`
   - Lists all config files
   - Displays their contents

---

## Question 5: Secret from File (7 minutes)

**Task**:
1. Create files:
   - `username.txt` containing `admin`
   - `password.txt` containing `securepass123`
2. Create secret `app-secret` from these files
3. Create pod `secret-pod` that uses these as env vars:
   - `USER` from `username.txt`
   - `PASS` from `password.txt`

---

## Question 6: Deployment Rollout Strategy (9 minutes)

**Task**: Update deployment `critical-app` (currently 5 replicas):
1. Configure RollingUpdate strategy:
   - maxSurge: 2
   - maxUnavailable: 1
2. Update image from `nginx:1.20` to `nginx:1.22`
3. Monitor rollout progress
4. Verify all pods are updated

---

## Question 7: StorageClass and Dynamic Provisioning (10 minutes)

**Task**:
1. Create StorageClass `fast-ssd`:
   - Provisioner: `kubernetes.io/no-provisioner`
   - Volume binding: `WaitForFirstConsumer`
   - Reclaim policy: `Delete`
2. Create PVC `dynamic-pvc` using this StorageClass (10Gi)
3. Create pod `storage-app` using the PVC at `/data`

---

## Question 8: LimitRange Configuration (8 minutes)

**Task**: Create LimitRange in namespace `dev` with:
- Default CPU request: 100m
- Default CPU limit: 200m
- Default memory request: 128Mi
- Default memory limit: 256Mi
- Max CPU per container: 2 cores
- Max memory per container: 4Gi
- Min CPU per container: 50m
- Min memory per container: 64Mi

---

## Question 9: ClusterRole and ClusterRoleBinding (9 minutes)

**Task**:
1. Create ServiceAccount `cluster-admin-sa`
2. Create ClusterRole `node-viewer` that allows:
   - `get`, `list` on nodes
   - `get`, `list` on namespaces
3. Create ClusterRoleBinding binding the SA to the ClusterRole
4. Test permissions

---

## Question 10: Advanced NetworkPolicy (12 minutes)

**Task**: Create NetworkPolicy for namespace `secure`:
1. Default deny all ingress and egress
2. Allow pods with label `app=web` to:
   - Receive ingress on port 80 from any pod in namespace
   - Send egress to pods with label `app=api` on port 8080
   - Send egress to external IP `8.8.8.8` on port 53 (DNS)
3. Allow pods with label `app=api` to:
   - Receive ingress from `app=web` pods on port 8080
   - Send egress to pods with label `app=db` on port 3306

---

## Question 11: Ingress with TLS (10 minutes)

**Task**: Create Ingress `secure-ingress`:
1. Host: `secure.example.com`
2. TLS secret: `tls-secret` (create dummy secret)
3. Routes:
   - `/` to `web-service:80`
   - `/api` to `api-service:8080`
4. Use ingress class `nginx`

---

## Question 12: StatefulSet Update Strategy (11 minutes)

**Task**: 
1. Create StatefulSet `app-sts` with 5 replicas
2. Configure partition-based update:
   - Set partition to 3
3. Update image to new version
4. Verify only pods 0, 1, 2 update
5. Update remaining pods by setting partition to 0

---

## Question 13: Job with Parallelism (8 minutes)

**Task**: Create Job `parallel-processor`:
- Completions: 10
- Parallelism: 3
- Image: `busybox:1.35`
- Command: `echo "Processing item $JOB_COMPLETION_INDEX" && sleep 10`
- Use indexed completion mode

---

## Question 14: CronJob Management (7 minutes)

**Task**:
1. Create CronJob `cleanup-job`:
   - Schedule: Every hour at minute 0
   - Concurrency policy: `Forbid`
   - Keep 5 successful jobs
2. Suspend the CronJob
3. Manually trigger a job from the CronJob
4. Resume the CronJob

---

## Question 15: DaemonSet with Node Selector (8 minutes)

**Task**: Create DaemonSet `monitoring-agent`:
- Runs only on nodes with label `node-type=compute`
- Image: `busybox:1.35`
- Command: `sh -c 'echo "Monitoring" && sleep 3600'`
- Has tolerations for tainted nodes

---

## Question 16: Node Taints and Tolerations (9 minutes)

**Task**:
1. Taint node `worker-1` with:
   - Key: `dedicated`
   - Value: `special`
   - Effect: `NoSchedule`
2. Create pod `special-pod` with matching toleration
3. Verify pod schedules on tainted node
4. Remove taint from node

---

## Question 17: Pod Affinity (10 minutes)

**Task**: 
1. Create deployment `cache` with 2 replicas (label: `app=cache`)
2. Create deployment `app` with 3 replicas that:
   - Prefers to run on same node as `cache` pods
   - Uses pod affinity with `preferredDuringScheduling`
   - Topology key: `kubernetes.io/hostname`
3. Verify pod placement

---

## Question 18: Complete Troubleshooting Scenario (15 minutes)

**Scenario**: Production application is down. Issues:
1. Deployment `web-app` has 0 ready replicas
2. Service `web-svc` has no endpoints
3. Pods are in `ImagePullBackOff` state
4. Node `worker-2` is `NotReady`

**Tasks**:
1. Diagnose all issues
2. Fix each problem
3. Verify application is working
4. Document what was wrong

---

## Scoring Guide

- **Question 1**: 8 points
- **Question 2**: 7 points
- **Question 3**: 6 points
- **Question 4**: 7 points
- **Question 5**: 6 points
- **Question 6**: 8 points
- **Question 7**: 9 points
- **Question 8**: 7 points
- **Question 9**: 8 points
- **Question 10**: 11 points
- **Question 11**: 9 points
- **Question 12**: 10 points
- **Question 13**: 7 points
- **Question 14**: 6 points
- **Question 15**: 7 points
- **Question 16**: 8 points
- **Question 17**: 9 points
- **Question 18**: 13 points

**Total**: 125 points  
**Passing**: 83 points (66%) | 93 points (74% for CKA)

---

## Time Management

- Average: ~6.5 minutes per question
- Complex questions (10, 12, 18): 15 minutes
- Simple questions (3, 5): 5-7 minutes
- Leave 10 minutes at end for review

Good luck! 🚀

