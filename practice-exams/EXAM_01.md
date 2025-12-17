# CKA Practice Exam 1

**Duration**: 2 hours  
**Total Tasks**: 17  
**Passing Score**: 74% (66 points minimum)  
**Exam Format**: Performance-based (hands-on tasks)

---

## Task 1: Pod Troubleshooting (10 minutes)
**Domain**: Troubleshooting (30% weight)

**Context**:  
A developer reports that their application pod is continuously crashing. You need to diagnose and fix the issue.

**Scenario**:  
A pod named `web-app` in the `production` namespace is in `CrashLoopBackOff` state.

**Task**:  
1. Identify the root cause of the pod failure
2. Fix the pod configuration to resolve the issue
3. Verify the pod is running successfully

**Verification Steps**:
```bash
# Verify pod is running
kubectl get pod web-app -n production
# Should show: STATUS = Running
```

**Hints** (if needed):
- Check pod logs: `kubectl logs web-app -n production --previous`
- Review pod events: `kubectl describe pod web-app -n production`
- Check resource limits and requests
- Verify image name and pull policy

---

## Task 2: Create Deployment (8 minutes)
**Domain**: Workloads & Scheduling (15% weight)

**Context**:  
You need to deploy a new API server application with specific resource requirements.

**Task**:  
Create a deployment named `api-server` in the `default` namespace with the following specifications:
- Image: `nginx:1.21`
- Replicas: 3
- Resource requests: CPU 100m, Memory 128Mi
- Resource limits: CPU 200m, Memory 256Mi
- Labels: `app=api` and `tier=backend` (on both deployment and pods)

**Verification Steps**:
```bash
# Verify deployment created
kubectl get deployment api-server
# Verify pods are running
kubectl get pods -l app=api,tier=backend
# Verify resource limits
kubectl describe deployment api-server | grep -A 5 Resources
```

---

## Task 3: Service Configuration (7 minutes)
**Domain**: Services & Networking (20% weight)

**Context**:  
The API server deployment needs to be accessible within the cluster via a service.

**Task**:  
Create a ClusterIP service named `api-service` in the `default` namespace that:
- Selects pods with labels `app=api` and `tier=backend`
- Exposes port 80 (service port)
- Maps to container port 8080 (target port)
- Verify the service has endpoints

**Verification Steps**:
```bash
# Verify service created
kubectl get svc api-service
# Verify endpoints exist
kubectl get endpoints api-service
# Should show 3 endpoints (one per pod)
kubectl describe svc api-service
```

---

## Task 4: ConfigMap and Pod (10 minutes)

1. Create a ConfigMap named `app-config` in the `default` namespace with:
   - `database_url`: `postgresql://db:5432/mydb`
   - `log_level`: `info`
   - `max_connections`: `100`

2. Create a pod named `config-pod` that:
   - Uses image `busybox:1.35`
   - Mounts the ConfigMap as a volume at `/etc/config`
   - Runs command: `sh -c 'cat /etc/config/* && sleep 3600'`

---

## Task 5: Secret Management (8 minutes)

1. Create a secret named `db-credentials` in the `default` namespace with:
   - `username`: `admin`
   - `password`: `secret123`

2. Create a pod named `db-client` that uses these secrets as environment variables:
   - `DB_USER` from `username` key
   - `DB_PASS` from `password` key

---

## Task 6: PersistentVolume and PVC (12 minutes)

1. Create a PersistentVolume named `pv-manual` with:
   - Storage: 5Gi
   - Access mode: ReadWriteOnce
   - Storage class: `manual`
   - Host path: `/mnt/data`

2. Create a PersistentVolumeClaim named `data-pvc` that:
   - Requests 3Gi storage
   - Uses storage class `manual`
   - Access mode: ReadWriteOnce

3. Verify the PVC is bound to the PV

---

## Task 7: Deployment Update (10 minutes)

Update the `api-server` deployment to:
- Use image `nginx:1.22`
- Scale to 5 replicas
- Monitor the rollout
- If the new version has issues, rollback to the previous version

---

## Task 8: RBAC Configuration (12 minutes)

1. Create a ServiceAccount named `monitor-sa` in the `monitoring` namespace

2. Create a Role named `pod-reader` in the `monitoring` namespace that allows:
   - `get`, `list`, `watch` on pods
   - `get`, `list` on services

3. Create a RoleBinding named `monitor-binding` that:
   - Binds the `pod-reader` role to `monitor-sa`
   - In the `monitoring` namespace

4. Verify the ServiceAccount has the correct permissions

---

## Task 9: NetworkPolicy (15 minutes)

Create NetworkPolicies in the `production` namespace:

1. Default deny all ingress and egress traffic

2. Allow `frontend` pods to:
   - Receive traffic on port 80
   - Send traffic to `backend` pods on port 8080

3. Allow `backend` pods to:
   - Receive traffic from `frontend` pods on port 8080
   - Send traffic to `database` pods on port 3306

4. Allow `database` pods to:
   - Receive traffic from `backend` pods on port 3306

---

## Task 10: Ingress Configuration (10 minutes)

Create an Ingress resource named `web-ingress` in the `default` namespace that:
- Uses ingress class `nginx`
- Routes traffic for host `app.example.com` to service `api-service` on port 80
- Routes `/api` path to service `api-service` on port 80
- Routes `/web` path to service `web-service` on port 80

---

## Task 11: StatefulSet (15 minutes)

Create a StatefulSet named `db-statefulset` in the `default` namespace:

1. Create a headless service named `db-headless` with:
   - ClusterIP: None
   - Selector: `app=database`
   - Port: 3306

2. Create StatefulSet with:
   - 3 replicas
   - Image: `mysql:8.0`
   - Environment variables: `MYSQL_ROOT_PASSWORD=rootpass`, `MYSQL_DATABASE=mydb`
   - Volume claim template: 10Gi, ReadWriteOnce, storage class `fast-ssd`

3. Verify all pods are running

---

## Task 12: Job and CronJob (10 minutes)

1. Create a Job named `data-backup` that:
   - Uses image `busybox:1.35`
   - Runs command: `echo "Backup completed" && date`
   - Has a deadline of 5 minutes
   - Retries up to 3 times

2. Create a CronJob named `daily-backup` that:
   - Runs daily at 2 AM
   - Uses the same image and command as the Job
   - Keeps 3 successful job history
   - Keeps 1 failed job history

---

## Task 13: DaemonSet (8 minutes)

Create a DaemonSet named `log-collector` in the `default` namespace that:
- Runs on all nodes (including master/control-plane)
- Uses image `busybox:1.35`
- Runs command: `sh -c 'echo "Collecting logs" && sleep 3600'`
- Has appropriate tolerations for master nodes

---

## Task 14: ResourceQuota (10 minutes)

Create a ResourceQuota named `compute-quota` in the `production` namespace that limits:
- CPU requests: 10 cores
- CPU limits: 20 cores
- Memory requests: 20Gi
- Memory limits: 40Gi
- Pods: 50
- PersistentVolumeClaims: 10

---

## Task 15: Node Maintenance (12 minutes)

Perform maintenance on node `worker-node-1`:

1. Cordon the node
2. Drain the node (ignore DaemonSets, delete emptyDir data)
3. Verify all pods have been rescheduled
4. Simulate maintenance (just document what you would do)
5. Uncordon the node
6. Verify the node is ready

---

## Task 16: Troubleshooting Service (10 minutes)

A service named `web-svc` in the `default` namespace has no endpoints. Diagnose and fix:

1. Check service selector
2. Check pod labels
3. Fix any mismatches
4. Verify endpoints are created

---

## Task 17: Pod Security Context (10 minutes)

Create a pod named `secure-app` in the `default` namespace that:
- Runs as non-root user (UID 1000)
- Has read-only root filesystem
- Drops all capabilities except `NET_BIND_SERVICE`
- Uses seccomp profile `RuntimeDefault`
- Has writable `/tmp` directory via emptyDir volume

---

## Scoring

- **Task 1**: 6 points
- **Task 2**: 5 points
- **Task 3**: 4 points
- **Task 4**: 6 points
- **Task 5**: 5 points
- **Task 6**: 7 points
- **Task 7**: 6 points
- **Task 8**: 7 points
- **Task 9**: 9 points
- **Task 10**: 6 points
- **Task 11**: 9 points
- **Task 12**: 6 points
- **Task 13**: 5 points
- **Task 14**: 6 points
- **Task 15**: 7 points
- **Task 16**: 6 points
- **Task 17**: 6 points

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

## Time Management Tips

- Spend no more than 10-15 minutes per task
- If stuck, move on and come back
- Verify your work quickly
- Use `kubectl explain` for help
- Check `kubectl get events` for troubleshooting

---

**Good luck!** 🚀

