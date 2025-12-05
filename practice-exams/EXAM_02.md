# CKA Practice Exam 2

**Duration**: 2 hours  
**Total Tasks**: 18  
**Passing Score**: 66% (74% for CKA)

---

## Task 1: Multi-Container Pod (12 minutes)

Create a pod named `app-sidecar` with:
- Main container: `nginx:1.21` on port 80
- Sidecar container: `busybox:1.35` that tails logs from shared volume
- Init container: Waits for main container to be ready
- Shared volume: `emptyDir` mounted at `/shared`

---

## Task 2: Deployment with Health Probes (10 minutes)

Create a deployment named `web-deployment` with:
- 3 replicas
- Image: `nginx:1.21`
- Readiness probe: HTTP GET on `/` every 10 seconds
- Liveness probe: HTTP GET on `/` every 20 seconds
- Startup probe: HTTP GET on `/` with 30 failure threshold

---

## Task 3: Service with Session Affinity (8 minutes)

Create a service named `sticky-service` that:
- Selects pods with label `app=web`
- Uses session affinity (ClientIP)
- Session timeout: 3 hours
- Exposes port 80

---

## Task 4: ConfigMap with envFrom (10 minutes)

1. Create ConfigMap `app-settings` with keys: `ENV=production`, `DEBUG=false`, `TIMEOUT=30`

2. Create a pod that uses `envFrom` to load all ConfigMap keys as environment variables

---

## Task 5: Secret from File (10 minutes)

1. Create files: `username.txt` (content: `admin`) and `password.txt` (content: `secret123`)

2. Create a secret `file-secret` from these files

3. Create a pod that mounts this secret as a volume at `/etc/secrets`

---

## Task 6: StorageClass and Dynamic Provisioning (12 minutes)

1. Create StorageClass `fast-ssd` with:
   - Provisioner: `kubernetes.io/no-provisioner`
   - Volume binding mode: `WaitForFirstConsumer`
   - Reclaim policy: `Delete`

2. Create a PVC `dynamic-pvc` using this StorageClass requesting 10Gi

3. Create a pod that uses this PVC

---

## Task 7: Deployment Rollout Strategy (10 minutes)

Update deployment `web-deployment` to use:
- Rolling update strategy
- Max surge: 2 pods
- Max unavailable: 1 pod
- Update image to `nginx:1.22`
- Monitor the rollout

---

## Task 8: ClusterRole and ClusterRoleBinding (12 minutes)

1. Create ClusterRole `node-viewer` that allows:
   - `get`, `list` on nodes
   - `get`, `list` on namespaces

2. Create ClusterRoleBinding that binds this role to ServiceAccount `cluster-monitor` in namespace `monitoring`

3. Verify permissions

---

## Task 9: NetworkPolicy with IP Block (15 minutes)

Create NetworkPolicy `external-api-policy` that:
- Applies to pods with label `app=api`
- Allows ingress from IP range `10.0.0.0/8` on port 8080
- Allows ingress from pods with label `app=frontend` on port 8080
- Denies all other ingress

---

## Task 10: Ingress with TLS (12 minutes)

Create an Ingress `secure-ingress` that:
- Uses ingress class `nginx`
- Host: `secure.example.com`
- TLS secret: `tls-secret`
- Routes to service `web-service` on port 80

Note: You don't need to create the TLS secret, just reference it.

---

## Task 11: StatefulSet Update Strategy (12 minutes)

Create StatefulSet `app-statefulset` with:
- 5 replicas
- Image: `nginx:1.21`
- Update strategy: RollingUpdate with partition 3
- Update image to `nginx:1.22` and verify only pods 0, 1, 2 update
- Then update remaining pods

---

## Task 12: Job with Parallelism (10 minutes)

Create a Job `parallel-processor` that:
- Completes 10 times
- Runs 3 pods in parallel
- Uses image `busybox:1.35`
- Command: `echo "Processing item $JOB_COMPLETION_INDEX" && sleep 10`
- Uses indexed completion mode

---

## Task 13: CronJob with Concurrency (10 minutes)

Create CronJob `hourly-sync` that:
- Runs every hour
- Concurrency policy: `Forbid` (don't run if previous job still running)
- Uses image `busybox:1.35`
- Command: `echo "Sync completed" && date`
- Keeps 5 successful jobs

---

## Task 14: DaemonSet with Node Selector (10 minutes)

Create DaemonSet `compute-monitor` that:
- Only runs on nodes with label `node-type=compute`
- Uses image `busybox:1.35`
- Command: `sh -c 'echo "Monitoring" && sleep 3600'`
- Has appropriate tolerations

---

## Task 15: LimitRange (10 minutes)

Create LimitRange `compute-limits` in namespace `production` that:
- Default CPU request: 100m, limit: 200m
- Default memory request: 128Mi, limit: 256Mi
- Max CPU per container: 2 cores
- Max memory per container: 4Gi
- Min CPU per container: 50m
- Min memory per container: 64Mi

---

## Task 16: Pod Disruption Budget (10 minutes)

Create PodDisruptionBudget `web-pdb` that:
- Applies to pods with label `app=web`
- Ensures minimum 3 pods available
- Works with deployment `web-deployment` (5 replicas)

---

## Task 17: Troubleshooting Deployment (12 minutes)

A deployment `api-deployment` is stuck and not updating. Diagnose:
1. Check rollout status
2. Check ReplicaSet status
3. Check pod status
4. Check events
5. Fix the issue

---

## Task 18: Node Labeling and Taints (10 minutes)

1. Label node `worker-1` with `zone=east` and `node-type=compute`

2. Taint the node with:
   - Key: `dedicated`
   - Value: `special`
   - Effect: `NoSchedule`

3. Create a pod with matching toleration that can schedule on this node

---

## Scoring

- **Task 1**: 7 points
- **Task 2**: 6 points
- **Task 3**: 5 points
- **Task 4**: 6 points
- **Task 5**: 6 points
- **Task 6**: 7 points
- **Task 7**: 6 points
- **Task 8**: 7 points
- **Task 9**: 9 points
- **Task 10**: 7 points
- **Task 11**: 7 points
- **Task 12**: 6 points
- **Task 13**: 6 points
- **Task 14**: 6 points
- **Task 15**: 6 points
- **Task 16**: 6 points
- **Task 17**: 7 points
- **Task 18**: 6 points

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

**Good luck!** 🚀

