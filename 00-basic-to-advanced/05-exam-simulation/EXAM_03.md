# CKA Practice Exam 3

**Time Limit**: 2 hours  
**Total Questions**: 16  
**Passing Score**: 66% (74% for actual CKA)

---

## Instructions

- Work efficiently and verify each solution
- Use documentation when needed
- Time management is critical
- All tasks in `default` namespace unless specified

---

## Question 1: Pod Security Context (9 minutes)

**Task**: Create pod `secure-app` with:
- Image: `nginx:1.21`
- Run as non-root user (UID 1000, GID 2000)
- Read-only root filesystem
- Drop all capabilities, add only `NET_BIND_SERVICE`
- Seccomp profile: `RuntimeDefault`

---

## Question 2: Deployment with Environment Variables (8 minutes)

**Task**: Create deployment `env-app`:
- Image: `busybox:1.35`
- Replicas: 2
- Environment variables from ConfigMap `app-config`:
  - `ENV` from key `environment`
  - `LOG_LEVEL` from key `log_level`
- Environment variables from Secret `app-secret`:
  - `API_KEY` from key `api_key`
- Use `envFrom` for all ConfigMap keys with prefix `CONFIG_`

---

## Question 3: Service Endpoints (7 minutes)

**Scenario**: Service `external-db` needs to point to external database at `192.168.1.100:3306`.

**Task**:
1. Create service `external-db` (no selector)
2. Create Endpoints object pointing to external IP
3. Test connectivity from a pod

---

## Question 4: Headless Service (6 minutes)

**Task**:
1. Create headless service `stateful-headless`
2. Create 3 pods with label `app=stateful`
3. Test DNS resolution (should return all 3 pod IPs)
4. Verify using `nslookup` from a pod

---

## Question 5: ConfigMap Immutable (6 minutes)

**Task**:
1. Create ConfigMap `immutable-config` with `immutable: true`
2. Add key `setting=value1`
3. Try to update it (should fail)
4. Explain why immutable ConfigMaps are useful

---

## Question 6: Secret Types (8 minutes)

**Task**: Create three different secret types:
1. Generic secret `app-secret` with `key=value`
2. Docker registry secret `regcred` for `registry.example.com`
3. TLS secret `tls-cert` (use dummy cert/key files)
4. Verify each secret type

---

## Question 7: PersistentVolume with Access Modes (9 minutes)

**Task**:
1. Create PV `shared-pv`:
   - 10Gi storage
   - AccessMode: `ReadWriteMany`
   - StorageClass: `shared`
   - HostPath: `/mnt/shared`
2. Create PVC `shared-pvc` matching the PV
3. Create 2 pods using the same PVC simultaneously
4. Verify both can write to the volume

---

## Question 8: ResourceQuota with Scopes (8 minutes)

**Task**: Create ResourceQuota in namespace `limited`:
- CPU requests: 4 cores
- Memory requests: 8Gi
- CPU limits: 8 cores
- Memory limits: 16Gi
- Pods: 20
- Scope: `NotTerminating` (only non-terminating pods)
- Scope: `BestEffort` (pods without resource requests)

---

## Question 9: RBAC with Resource Names (9 minutes)

**Task**:
1. Create Role `specific-pod-role` that allows:
   - `get`, `update` on pod `web-pod` only
   - `list` on all pods
2. Create RoleBinding for ServiceAccount `pod-manager-sa`
3. Test: Can get `web-pod`, cannot get other pods, can list all pods

---

## Question 10: NetworkPolicy with IP Blocks (11 minutes)

**Task**: Create NetworkPolicy for pods with label `app=api`:
1. Allow ingress from:
   - Pods with label `app=web` on port 8080
   - IP block `10.0.0.0/8` on port 443
   - IP block `172.16.0.0/12` except `172.16.1.0/24` on port 443
2. Allow egress to:
   - Pods with label `app=db` on port 3306
   - External DNS `8.8.8.8` on port 53

---

## Question 11: Ingress with Rewrite (9 minutes)

**Task**: Create Ingress `rewrite-ingress`:
- Host: `app.example.com`
- Path `/api/v1` rewrites to `/` for service `api-service:8080`
- Path `/web` rewrites to `/` for service `web-service:80`
- Use nginx ingress controller annotations

---

## Question 12: StatefulSet with Init Containers (10 minutes)

**Task**: Create StatefulSet `db-sts`:
- 3 replicas
- Init container: Wait for service `db-service` to be available
- Main container: `mysql:8.0` with env vars
- Persistent storage: 10Gi per pod
- Headless service

---

## Question 13: Job with TTL (7 minutes)

**Task**: Create Job `ttl-job`:
- Image: `busybox:1.35`
- Command: `echo "Job done" && sleep 30`
- TTL: 100 seconds (auto-delete after completion)
- Verify job is deleted after completion

---

## Question 14: CronJob with Deadlines (8 minutes)

**Task**: Create CronJob `long-running`:
- Schedule: `*/5 * * * *` (every 5 minutes)
- Active deadline: 2 minutes
- Starting deadline: 1 minute
- Concurrency: `Allow`
- Verify it runs and completes

---

## Question 15: Pod Disruption Budget (8 minutes)

**Task**: 
1. Create deployment `critical-app` with 6 replicas
2. Create PDB ensuring:
   - Minimum 4 pods available
   - Maximum 2 pods unavailable
3. Test by draining a node
4. Verify PDB prevents too many pods from being evicted

---

## Question 16: Complete Application Deployment (20 minutes)

**Task**: Deploy a complete 3-tier application:

1. **Database Layer**:
   - StatefulSet `db-sts` with 2 replicas
   - Headless service `db-headless`
   - Persistent storage 5Gi per pod
   - Image: `mysql:8.0` with env vars

2. **Backend Layer**:
   - Deployment `api-deploy` with 3 replicas
   - Service `api-service` ClusterIP on port 8080
   - ConfigMap `api-config` with database connection
   - Health probes configured

3. **Frontend Layer**:
   - Deployment `web-deploy` with 2 replicas
   - Service `web-service` NodePort on port 80
   - Ingress routing `app.example.com` to web-service

4. **Security**:
   - NetworkPolicy allowing frontend → backend → database
   - RBAC: ServiceAccount with minimal permissions

5. **Verification**:
   - All pods running
   - Services have endpoints
   - Ingress routes correctly
   - Network policies work

---

## Scoring Guide

- **Question 1**: 8 points
- **Question 2**: 7 points
- **Question 3**: 6 points
- **Question 4**: 5 points
- **Question 5**: 5 points
- **Question 6**: 7 points
- **Question 7**: 8 points
- **Question 8**: 7 points
- **Question 9**: 8 points
- **Question 10**: 10 points
- **Question 11**: 8 points
- **Question 12**: 9 points
- **Question 13**: 6 points
- **Question 14**: 7 points
- **Question 15**: 7 points
- **Question 16**: 18 points

**Total**: 122 points  
**Passing**: 81 points (66%) | 90 points (74% for CKA)

---

## Time Management

- Questions 1-15: ~8 minutes each (120 minutes)
- Question 16: 20 minutes
- Review: 10 minutes
- Total: 150 minutes (2.5 hours - adjust as needed)

Focus on accuracy over speed! 🚀

