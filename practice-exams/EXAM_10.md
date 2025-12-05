# CKA Practice Exam 10 - Full Exam Simulation

**Duration**: 2 hours  
**Total Tasks**: 20  
**Passing Score**: 66% (74% for CKA)

**This is the most realistic exam simulation. Treat it like the real CKA exam.**

---

## Task 1: Pod Troubleshooting (8 minutes)

Pod `web-app` in namespace `production` is in `CrashLoopBackOff`. Diagnose and fix.

---

## Task 2: Create Deployment (6 minutes)

Create deployment `api-deployment`:
- Image: `nginx:1.21`
- Replicas: 3
- Labels: `app=api`, `tier=backend`
- Resources: CPU 100m/200m, Memory 128Mi/256Mi

---

## Task 3: Service Configuration (6 minutes)

Create ClusterIP service `api-service`:
- Selects: `app=api`, `tier=backend`
- Port: 80 → 8080
- Verify endpoints

---

## Task 4: ConfigMap and Pod (8 minutes)

1. Create ConfigMap `app-config` with `env=prod`, `debug=false`
2. Create pod using ConfigMap as volume at `/etc/config`

---

## Task 5: Secret Management (6 minutes)

1. Create secret `db-secret` with `user=admin`, `pass=secret123`
2. Use in pod as environment variables

---

## Task 6: PersistentVolume (10 minutes)

1. Create PV `pv-manual` (10Gi, RWO, manual)
2. Create PVC `data-pvc` (5Gi, manual)
3. Verify binding

---

## Task 7: Deployment Update (8 minutes)

Update `api-deployment`:
- Image: `nginx:1.22`
- Scale: 5 replicas
- Monitor rollout
- Rollback if issues

---

## Task 8: RBAC Setup (10 minutes)

1. Create ServiceAccount `monitor-sa` in `monitoring` namespace
2. Create Role `pod-reader` (get, list, watch pods)
3. Create RoleBinding
4. Verify permissions

---

## Task 9: NetworkPolicy (12 minutes)

In namespace `production`:
1. Default deny all
2. Allow `app=web` to receive on port 80
3. Allow `app=web` to send to `app=api` on port 8080

---

## Task 10: Ingress (8 minutes)

Create Ingress `web-ingress`:
- Class: `nginx`
- Host: `app.example.com`
- Routes to `web-service:80`

---

## Task 11: StatefulSet (12 minutes)

Create StatefulSet `db-sts`:
- 3 replicas
- Headless service `db-headless`
- Image: `mysql:8.0` with env vars
- Storage: 10Gi per pod

---

## Task 12: Job (6 minutes)

Create Job `backup-job`:
- Image: `busybox:1.35`
- Command: `echo "Backup done" && date`
- Deadline: 5 minutes
- Retries: 3

---

## Task 13: CronJob (6 minutes)

Create CronJob `daily-backup`:
- Schedule: `0 2 * * *`
- Same command as Job
- Keep 3 successful jobs

---

## Task 14: DaemonSet (6 minutes)

Create DaemonSet `log-collector`:
- Runs on all nodes
- Image: `busybox:1.35`
- Command: `echo "Collecting" && sleep 3600`
- Tolerations for master nodes

---

## Task 15: ResourceQuota (8 minutes)

Create ResourceQuota `team-quota` in `production`:
- CPU: 10 cores request, 20 cores limit
- Memory: 20Gi request, 40Gi limit
- Pods: 50

---

## Task 16: Node Maintenance (10 minutes)

1. Cordon `worker-1`
2. Drain `worker-1` (ignore DaemonSets)
3. Verify pods rescheduled
4. Uncordon `worker-1`

---

## Task 17: Service Troubleshooting (8 minutes)

Service `web-svc` has no endpoints. Fix:
- Check selector
- Check pod labels
- Fix mismatch
- Verify endpoints

---

## Task 18: Pod Security (8 minutes)

Create pod `secure-pod`:
- Non-root (UID 1000)
- Read-only root filesystem
- Drop all capabilities
- Add `NET_BIND_SERVICE`
- Writable `/tmp`

---

## Task 19: StorageClass (8 minutes)

Create StorageClass `fast-ssd`:
- Provisioner: `kubernetes.io/no-provisioner`
- Binding: `WaitForFirstConsumer`
- Reclaim: `Delete`
- Set as default

---

## Task 20: Complete Application (12 minutes)

Deploy complete app:
1. Deployment (3 replicas) with health probes
2. Service (ClusterIP)
3. ConfigMap
4. Secret
5. Ingress
6. Verify end-to-end

---

## Scoring

- **Tasks 1-5**: 5 points each (25 points)
- **Tasks 6-10**: 5 points each (25 points)
- **Tasks 11-15**: 5 points each (25 points)
- **Tasks 16-20**: 5 points each (25 points)

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

## Exam Day Tips

1. **Read all tasks first** - Prioritize by difficulty
2. **Time box each task** - Don't spend > 10 min on any task
3. **Verify your work** - Quick check before moving on
4. **Use kubectl explain** - It's allowed!
5. **Stay calm** - If stuck, move on and come back

---

**This is your final practice. Good luck!** 🚀

