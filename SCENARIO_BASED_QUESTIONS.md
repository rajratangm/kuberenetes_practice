# Scenario-Based CKA Practice Questions

These questions simulate real CKA exam scenarios, from basic to advanced difficulty. Each scenario is designed to test practical Kubernetes administration skills.

## Basic Scenarios (Foundation Level)

### Scenario 1: Simple Pod Deployment Issue
**Situation**: A developer reports that their pod `app-pod` is not starting. You need to diagnose and fix it.

**Given**:
- Pod name: `app-pod`
- Namespace: `default`
- Pod is in `Pending` state

**Tasks**:
1. Identify why the pod is pending
2. Fix the issue
3. Verify the pod is running

**Expected Time**: 3-5 minutes

**Hints**:
- Check pod events
- Verify node resources
- Check node selectors/taints

---

### Scenario 2: Service Not Accessible
**Situation**: A service `web-service` exists but applications cannot connect to it.

**Given**:
- Service: `web-service` in namespace `production`
- Service type: ClusterIP
- Port: 80

**Tasks**:
1. Verify service exists and is configured correctly
2. Check if endpoints are created
3. Verify pod labels match service selector
4. Test connectivity from a test pod

**Expected Time**: 4-6 minutes

**Common Issues to Check**:
- Service selector doesn't match pod labels
- Pods are not running
- Port mismatch
- NetworkPolicy blocking traffic

---

### Scenario 3: Deployment Not Scaling
**Situation**: A deployment `api-deployment` should have 5 replicas but only shows 3.

**Given**:
- Deployment: `api-deployment`
- Desired replicas: 5
- Current replicas: 3

**Tasks**:
1. Check why replicas are not scaling
2. Identify resource constraints
3. Fix the issue
4. Verify all 5 replicas are running

**Expected Time**: 5-7 minutes

**Common Causes**:
- ResourceQuota limits
- Node capacity
- Pod disruption budget
- Node taints

---

### Scenario 4: ConfigMap Not Loading
**Situation**: A pod cannot read configuration from a ConfigMap.

**Given**:
- Pod: `config-app`
- ConfigMap: `app-config`
- ConfigMap mounted at `/etc/config`

**Tasks**:
1. Verify ConfigMap exists
2. Check pod volume mount configuration
3. Verify ConfigMap keys
4. Fix and verify pod can read config

**Expected Time**: 4-6 minutes

---

### Scenario 5: Secret Access Denied
**Situation**: A pod needs database credentials but cannot access the secret.

**Given**:
- Pod: `db-client`
- Secret: `db-credentials`
- Secret should be available as environment variables

**Tasks**:
1. Verify secret exists
2. Check pod configuration
3. Fix secret reference
4. Verify pod can access credentials

**Expected Time**: 4-6 minutes

---

## Intermediate Scenarios (Practical Level)

### Scenario 6: Rolling Update Stuck
**Situation**: A deployment update is stuck and not completing the rollout.

**Given**:
- Deployment: `frontend`
- Image update: `nginx:1.21` → `nginx:1.22`
- Rollout status shows "progressing" for 10 minutes

**Tasks**:
1. Check rollout status and history
2. Identify why rollout is stuck
3. Check new ReplicaSet pod status
4. Fix the issue (image pull, resource limits, probes)
5. Complete or rollback the update

**Expected Time**: 6-8 minutes

**Common Issues**:
- ImagePullBackOff
- Readiness probe failing
- Resource limits too low
- Node capacity issues

---

### Scenario 7: Persistent Volume Not Binding
**Situation**: A PVC is stuck in `Pending` state and not binding to a PV.

**Given**:
- PVC: `data-pvc` requesting 10Gi
- StorageClass: `fast-ssd`
- PVC status: Pending

**Tasks**:
1. Check PVC details and requirements
2. Check available PVs
3. Verify StorageClass configuration
4. Create matching PV or fix StorageClass
5. Verify PVC binds successfully

**Expected Time**: 6-8 minutes

**Common Issues**:
- No matching PV available
- Access mode mismatch
- StorageClass not found
- Provisioner not working

---

### Scenario 8: NetworkPolicy Blocking Traffic
**Situation**: Pods cannot communicate after a NetworkPolicy was applied.

**Given**:
- NetworkPolicy: `web-policy` in namespace `production`
- Pods with label `app=web` cannot reach pods with label `app=api`

**Tasks**:
1. Review NetworkPolicy rules
2. Identify what traffic is blocked
3. Update NetworkPolicy to allow required traffic
4. Test connectivity between pods
5. Verify NetworkPolicy works correctly

**Expected Time**: 7-9 minutes

**Common Issues**:
- Missing ingress rules
- Missing egress rules
- Incorrect pod selectors
- Port restrictions

---

### Scenario 9: RBAC Permission Denied
**Situation**: A ServiceAccount cannot perform required operations.

**Given**:
- ServiceAccount: `api-sa` in namespace `development`
- Pod using this SA cannot list pods
- Error: "forbidden: User cannot list pods"

**Tasks**:
1. Check ServiceAccount exists
2. Review Role/RoleBinding configuration
3. Verify permissions granted
4. Fix RBAC configuration
5. Test permissions with `kubectl auth can-i`

**Expected Time**: 6-8 minutes

**Common Issues**:
- Role missing required verbs
- RoleBinding to wrong subject
- Wrong namespace
- Missing resource in role rules

---

### Scenario 10: Ingress Not Routing
**Situation**: Ingress resource exists but traffic is not reaching the backend service.

**Given**:
- Ingress: `web-ingress` routing to `web-service:80`
- Host: `app.example.com`
- Ingress controller is installed

**Tasks**:
1. Verify Ingress resource configuration
2. Check Ingress controller status
3. Verify backend service exists and has endpoints
4. Check Ingress status for IP/address
5. Test routing with curl

**Expected Time**: 7-9 minutes

**Common Issues**:
- Ingress controller not running
- Service selector mismatch
- Path configuration incorrect
- TLS configuration issues

---

### Scenario 11: StatefulSet Pod Not Starting
**Situation**: A StatefulSet pod is stuck in `Pending` state.

**Given**:
- StatefulSet: `db-statefulset` with 3 replicas
- Pod `db-statefulset-1` is Pending
- Other pods (0, 2) are running

**Tasks**:
1. Check pod events and status
2. Verify PVC binding for this pod
3. Check node capacity and taints
4. Fix the issue
5. Verify pod starts successfully

**Expected Time**: 6-8 minutes

**Common Issues**:
- PVC not bound
- Node resource constraints
- Node taints
- StorageClass issues

---

### Scenario 12: Job Not Completing
**Situation**: A Job is running but not completing successfully.

**Given**:
- Job: `data-processor`
- Status: Running for 30 minutes
- Expected completion: 5 minutes

**Tasks**:
1. Check job status and conditions
2. Review pod logs
3. Identify why job is not completing
4. Fix the issue (command, resources, image)
5. Verify job completes

**Expected Time**: 5-7 minutes

**Common Issues**:
- Command hanging
- Resource limits too low
- Missing dependencies
- Application errors

---

## Advanced Scenarios (Expert Level)

### Scenario 13: Multi-Namespace Service Discovery
**Situation**: A pod in namespace `production` needs to access a service in namespace `development`.

**Given**:
- Source pod: `prod-app` in namespace `production`
- Target service: `dev-api` in namespace `development`
- Connection fails with "connection refused"

**Tasks**:
1. Verify service exists in development namespace
2. Check service endpoints
3. Use FQDN for cross-namespace access
4. Verify NetworkPolicy allows cross-namespace traffic
5. Test connectivity

**Expected Time**: 8-10 minutes

**Key Points**:
- Use FQDN: `dev-api.development.svc.cluster.local`
- Check NetworkPolicy rules
- Verify service selector matches pods

---

### Scenario 14: Cluster Resource Exhaustion
**Situation**: New pods cannot be scheduled due to resource constraints.

**Given**:
- Multiple pods in `Pending` state
- Error: "Insufficient cpu" or "Insufficient memory"
- Cluster has 3 nodes

**Tasks**:
1. Check node resource capacity and allocation
2. Identify resource-hungry pods
3. Check ResourceQuotas in namespaces
4. Optimize resource requests/limits
5. Verify pods can be scheduled

**Expected Time**: 10-12 minutes

**Actions**:
- Review `kubectl top nodes` and `kubectl top pods`
- Check ResourceQuotas
- Adjust pod resource requests
- Consider adding nodes or evicting low-priority pods

---

### Scenario 15: etcd Backup and Restore Simulation
**Situation**: You need to backup etcd and understand the restore process.

**Given**:
- etcd running as static pod
- Need to create backup
- Understand restore procedure

**Tasks**:
1. Identify etcd pod and endpoints
2. Create etcd backup using etcdctl
3. Verify backup file integrity
4. Document restore procedure
5. (Optional) Test restore in test environment

**Expected Time**: 12-15 minutes

**Key Commands**:
```bash
# Find etcd pod
kubectl get pods -n kube-system | grep etcd

# Backup (example)
ETCDCTL_API=3 etcdctl snapshot save /backup/etcd-snapshot.db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key
```

---

### Scenario 16: Node Maintenance with Zero Downtime
**Situation**: You need to perform maintenance on a node without affecting running workloads.

**Given**:
- Node: `worker-node-1`
- Multiple pods running on this node
- Maintenance window: 30 minutes

**Tasks**:
1. Cordon the node to prevent new pods
2. Drain the node gracefully
3. Perform maintenance (simulated)
4. Uncordon the node
5. Verify pods rescheduled and running

**Expected Time**: 8-10 minutes

**Key Steps**:
- `kubectl cordon worker-node-1`
- `kubectl drain worker-node-1 --ignore-daemonsets --delete-emptydir-data`
- Wait for pods to reschedule
- `kubectl uncordon worker-node-1`

---

### Scenario 17: Deployment Rollback Under Pressure
**Situation**: A deployment update caused issues and needs immediate rollback.

**Given**:
- Deployment: `critical-app` with 10 replicas
- Recent update to image `app:v2.0` causing errors
- Need to rollback to `app:v1.0` quickly

**Tasks**:
1. Check deployment rollout history
2. Identify previous working revision
3. Perform rollback
4. Verify rollback success
5. Monitor pod health after rollback

**Expected Time**: 5-7 minutes

**Key Commands**:
- `kubectl rollout history deployment/critical-app`
- `kubectl rollout undo deployment/critical-app`
- `kubectl rollout status deployment/critical-app`

---

### Scenario 18: Pod Security Policy Violation
**Situation**: Pods cannot be created due to Pod Security Standards.

**Given**:
- Namespace: `restricted-ns` with restricted Pod Security Standard
- Pod creation fails with security policy violation
- Pod needs to run as non-root

**Tasks**:
1. Check namespace Pod Security Standard labels
2. Review pod security context requirements
3. Update pod with proper security context
4. Verify pod can be created
5. Test pod functionality

**Expected Time**: 7-9 minutes

**Requirements**:
- Run as non-root user
- Read-only root filesystem
- Drop all capabilities
- Proper volume mounts for writable directories

---

### Scenario 19: Service Mesh-like Configuration
**Situation**: Implement service-to-service communication with NetworkPolicies.

**Given**:
- Frontend pods: `app=frontend`
- Backend pods: `app=backend`
- Database pods: `app=database`
- Need to restrict: Frontend → Backend → Database only

**Tasks**:
1. Create NetworkPolicy for frontend (allow ingress from anywhere on port 80)
2. Create NetworkPolicy for backend (allow ingress from frontend only)
3. Create NetworkPolicy for database (allow ingress from backend only on port 3306)
4. Test connectivity chain
5. Verify isolation works

**Expected Time**: 10-12 minutes

**NetworkPolicy Rules**:
- Frontend: Allow ingress on 80 from anywhere
- Backend: Allow ingress on 8080 from pods with `app=frontend`
- Database: Allow ingress on 3306 from pods with `app=backend`

---

### Scenario 20: Multi-Container Pod Coordination
**Situation**: A pod has multiple containers that need to coordinate.

**Given**:
- Pod: `app-sidecar` with containers: `app` and `sidecar`
- Sidecar needs to start after app is ready
- Both share a volume at `/shared`

**Tasks**:
1. Create pod with init container to wait for app readiness
2. Configure shared volume
3. Set up proper startup order
4. Verify both containers run correctly
5. Test data sharing between containers

**Expected Time**: 8-10 minutes

**Key Configuration**:
- Init container checks app health
- Shared emptyDir volume
- Proper container startup commands

---

### Scenario 21: Resource Quota Exceeded
**Situation**: Cannot create new pods due to ResourceQuota limits.

**Given**:
- Namespace: `production` with ResourceQuota
- Error: "exceeded quota"
- Need to create new deployment

**Tasks**:
1. Check ResourceQuota limits and usage
2. Identify which resource is exhausted
3. Either increase quota or optimize existing resources
4. Create new deployment
5. Verify quota compliance

**Expected Time**: 8-10 minutes

**Actions**:
- `kubectl describe resourcequota -n production`
- Check current usage vs limits
- Delete unused resources or increase quota
- Optimize pod resource requests

---

### Scenario 22: CronJob Not Running
**Situation**: A CronJob is not creating Jobs as expected.

**Given**:
- CronJob: `backup-job` scheduled for `0 2 * * *`
- No Jobs created
- Need to diagnose and fix

**Tasks**:
1. Check CronJob configuration and status
2. Verify schedule syntax
3. Check if CronJob is suspended
4. Check controller manager logs (if accessible)
5. Manually trigger Job to test
6. Fix and verify CronJob works

**Expected Time**: 7-9 minutes

**Common Issues**:
- CronJob suspended
- Invalid schedule syntax
- Controller manager issues
- Resource constraints

---

### Scenario 23: DaemonSet Not Running on All Nodes
**Situation**: A DaemonSet should run on all nodes but is missing on some.

**Given**:
- DaemonSet: `log-collector`
- Expected: 1 pod per node (3 nodes = 3 pods)
- Actual: Only 2 pods running

**Tasks**:
1. Check DaemonSet status and desired/current pods
2. Check node labels and taints
3. Verify DaemonSet node selector
4. Check pod events for missing pod
5. Fix and verify DaemonSet runs on all nodes

**Expected Time**: 6-8 minutes

**Common Issues**:
- Node selector too restrictive
- Node taints without tolerations
- Node not ready
- Resource constraints

---

### Scenario 24: StatefulSet Scaling Issues
**Situation**: Cannot scale StatefulSet due to storage constraints.

**Given**:
- StatefulSet: `db-statefulset` with 3 replicas
- Trying to scale to 5 replicas
- New pods stuck in Pending (PVC issues)

**Tasks**:
1. Check StatefulSet status
2. Verify PVC creation for new replicas
3. Check StorageClass and available storage
4. Fix storage issues
5. Verify scaling completes

**Expected Time**: 8-10 minutes

**Common Issues**:
- StorageClass not found
- Insufficient storage in cluster
- PVC not binding
- StorageClass provisioner issues

---

### Scenario 25: Pod Disruption Budget Preventing Updates
**Situation**: Deployment update blocked by PodDisruptionBudget.

**Given**:
- Deployment: `web-app` with 5 replicas
- PDB: Min available 4 pods
- Rolling update cannot proceed

**Tasks**:
1. Check PDB configuration
2. Understand PDB interaction with rolling updates
3. Temporarily adjust PDB or update strategy
4. Complete deployment update
5. Restore PDB settings

**Expected Time**: 7-9 minutes

**Key Understanding**:
- PDB ensures minimum availability during voluntary disruptions
- Rolling updates respect PDB
- May need to adjust maxUnavailable in deployment

---

## Expert-Level Scenarios (Mastery Level)

### Scenario 26: Complete Cluster Troubleshooting
**Situation**: Multiple issues reported across the cluster.

**Given**:
- Node `worker-2` is NotReady
- Pods in `Pending` state
- Services not accessible
- Deployment rollouts failing

**Tasks**:
1. Prioritize issues by severity
2. Fix node NotReady issue
3. Resolve pod scheduling issues
4. Fix service connectivity
5. Resolve deployment issues
6. Verify cluster health

**Expected Time**: 15-20 minutes

**Approach**:
- Start with node issues (affects everything)
- Then pod scheduling
- Then service networking
- Finally application issues

---

### Scenario 27: Multi-Tier Application Deployment
**Situation**: Deploy complete 3-tier application with all best practices.

**Given**:
- Frontend: nginx serving static files
- Backend: API server
- Database: StatefulSet with persistent storage
- Requirements: Security, HA, monitoring

**Tasks**:
1. Create namespaces and RBAC
2. Deploy database with StatefulSet and PVCs
3. Deploy backend with ConfigMaps and Secrets
4. Deploy frontend with Ingress
5. Configure NetworkPolicies
6. Set up PDBs and ResourceQuotas
7. Verify end-to-end functionality

**Expected Time**: 20-25 minutes

**Best Practices**:
- Proper resource requests/limits
- Health probes
- Security contexts
- Network isolation
- High availability

---

### Scenario 28: Disaster Recovery Exercise
**Situation**: Simulate disaster recovery scenario.

**Given**:
- Need to backup critical resources
- Simulate namespace deletion
- Restore from backup

**Tasks**:
1. Export all resources from production namespace
2. Backup ConfigMaps, Secrets, Deployments, Services
3. Delete namespace (simulate disaster)
4. Restore all resources from backup
5. Verify application functionality

**Expected Time**: 15-18 minutes

**Key Commands**:
```bash
# Backup
kubectl get all,configmap,secret -n production -o yaml > backup.yaml

# Restore
kubectl apply -f backup.yaml
```

---

### Scenario 29: Performance Optimization
**Situation**: Cluster performance degradation, need to optimize.

**Given**:
- High CPU usage on nodes
- Pods being evicted
- Slow application response

**Tasks**:
1. Identify resource-hungry pods
2. Analyze resource requests vs actual usage
3. Optimize resource requests and limits
4. Check for resource leaks
5. Verify performance improvement

**Expected Time**: 12-15 minutes

**Tools**:
- `kubectl top nodes`
- `kubectl top pods`
- `kubectl describe pod` for resource usage
- ResourceQuota analysis

---

### Scenario 30: Security Hardening
**Situation**: Harden cluster security configuration.

**Given**:
- Namespace needs restricted Pod Security Standard
- Need NetworkPolicies for isolation
- RBAC for least privilege
- Secrets properly managed

**Tasks**:
1. Apply Pod Security Standards
2. Create comprehensive NetworkPolicies
3. Review and fix RBAC (principle of least privilege)
4. Audit Secret usage
5. Verify security configuration

**Expected Time**: 15-18 minutes

**Security Checklist**:
- Pod Security Standards enforced
- NetworkPolicies restrict traffic
- RBAC follows least privilege
- Secrets not in environment variables unnecessarily
- Security contexts on pods

---

## Exam Tips for Scenario-Based Questions

1. **Read Carefully**: Understand all requirements before starting
2. **Prioritize**: Fix critical issues first (nodes, then pods, then services)
3. **Verify**: Always verify your fixes work
4. **Time Management**: Don't spend too long on one scenario
5. **Use kubectl explain**: When unsure about YAML structure
6. **Check Events**: `kubectl get events` is your friend
7. **Test Connectivity**: Always test if services/pods can communicate
8. **Document**: Note what you did for verification

## Practice Strategy

1. **Start with Basics**: Master basic scenarios first
2. **Progress Gradually**: Move to intermediate, then advanced
3. **Time Yourself**: Practice under time pressure
4. **Repeat Scenarios**: Do each scenario multiple times
5. **Understand Why**: Don't just fix, understand the root cause
6. **Combine Scenarios**: Practice multiple scenarios together

---

## Real Exam-Style Scenarios (Based on Actual CKA Patterns)

### Scenario 31: Quick Fix - Pod CrashLoopBackOff
**Situation**: Pod `api-server` is in CrashLoopBackOff. Fix it quickly.

**Given**:
- Pod: `api-server` in namespace `default`
- Status: CrashLoopBackOff
- Time limit: 3 minutes

**Tasks**:
1. Get pod logs (including previous)
2. Identify the error
3. Fix the issue
4. Verify pod is running

**Common Fixes**:
- Wrong command in container
- Missing environment variable
- Incorrect image tag
- Resource limits too low

---

### Scenario 32: Service Endpoint Missing
**Situation**: Service `web-svc` has no endpoints. Fix it.

**Given**:
- Service: `web-svc`
- Endpoints: None
- Time limit: 4 minutes

**Tasks**:
1. Check service selector
2. Check pod labels
3. Fix mismatch
4. Verify endpoints created

**Quick Check**:
```bash
kubectl get svc web-svc -o yaml | grep selector
kubectl get pods --show-labels
```

---

### Scenario 33: Deployment Image Update
**Situation**: Update deployment `frontend` to use image `nginx:1.23`.

**Given**:
- Deployment: `frontend`
- Current image: `nginx:1.21`
- Time limit: 3 minutes

**Tasks**:
1. Update image
2. Monitor rollout
3. Verify all pods updated

**Commands**:
```bash
kubectl set image deployment/frontend nginx=nginx:1.23
kubectl rollout status deployment/frontend
```

---

### Scenario 34: Create ConfigMap and Use in Pod
**Situation**: Create ConfigMap `app-config` with key `env=production` and use it in pod `app-pod`.

**Given**:
- ConfigMap name: `app-config`
- Key: `env`, Value: `production`
- Pod: `app-pod` using image `busybox:1.35`
- Time limit: 5 minutes

**Tasks**:
1. Create ConfigMap
2. Create pod with ConfigMap as env var
3. Verify pod can read the value

**Commands**:
```bash
kubectl create configmap app-config --from-literal=env=production
# Create pod YAML with envFrom or env
kubectl exec app-pod -- env | grep env
```

---

### Scenario 35: RBAC - Allow Pod Creation
**Situation**: ServiceAccount `dev-sa` cannot create pods. Fix it.

**Given**:
- ServiceAccount: `dev-sa` in namespace `development`
- Error: "cannot create pods"
- Time limit: 6 minutes

**Tasks**:
1. Create Role with pod creation permission
2. Create RoleBinding
3. Test permission

**Quick Solution**:
```bash
kubectl create role pod-creator --verb=create --resource=pods -n development
kubectl create rolebinding pod-creator-binding --role=pod-creator --serviceaccount=development:dev-sa -n development
kubectl auth can-i create pods --as=system:serviceaccount:development:dev-sa -n development
```

---

### Scenario 36: PVC Not Binding - Quick Fix
**Situation**: PVC `data-pvc` is Pending. Make it bind.

**Given**:
- PVC: `data-pvc` requesting 5Gi
- Status: Pending
- Time limit: 5 minutes

**Tasks**:
1. Check PVC details
2. Check available PVs
3. Create matching PV or fix StorageClass
4. Verify binding

---

### Scenario 37: NetworkPolicy - Allow Specific Traffic
**Situation**: Create NetworkPolicy allowing only pods with label `role=frontend` to access pods with label `app=api` on port 8080.

**Given**:
- Target pods: `app=api`
- Allowed source: `role=frontend`
- Port: 8080
- Time limit: 7 minutes

**Tasks**:
1. Create NetworkPolicy YAML
2. Apply it
3. Test connectivity

---

### Scenario 38: Node Cordon and Drain
**Situation**: Prepare node `worker-1` for maintenance.

**Given**:
- Node: `worker-1`
- Time limit: 4 minutes

**Tasks**:
1. Cordon the node
2. Drain the node
3. Verify pods rescheduled

---

### Scenario 39: Deployment Rollback
**Situation**: Rollback deployment `web-app` to previous version.

**Given**:
- Deployment: `web-app`
- Current version has issues
- Time limit: 3 minutes

**Tasks**:
1. Check rollout history
2. Rollback to previous version
3. Verify rollback success

---

### Scenario 40: Secret Creation and Usage
**Situation**: Create secret `db-secret` with username and password, use in pod.

**Given**:
- Secret: `db-secret`
- Keys: `username=admin`, `password=secret123`
- Pod: `db-client`
- Time limit: 5 minutes

**Tasks**:
1. Create secret
2. Create pod using secret as env vars
3. Verify pod can access credentials

---

## Time-Pressured Scenarios (Exam Simulation)

### Scenario 41: Multiple Issues - Prioritize and Fix
**Situation**: You have 15 minutes to fix 3 issues:
1. Pod `app-1` in CrashLoopBackOff
2. Service `web-svc` has no endpoints
3. Deployment `api` rollout stuck

**Tasks**:
1. Prioritize issues
2. Fix each one
3. Verify all fixed

**Strategy**:
- Fix service first (affects connectivity)
- Fix deployment (affects availability)
- Fix pod last (may be symptom of other issues)

---

### Scenario 42: Complete Application Setup
**Situation**: Deploy complete app in 20 minutes:
- Deployment with 3 replicas
- Service exposing it
- ConfigMap with configuration
- Ingress routing traffic

**Tasks**:
1. Create all resources
2. Verify everything works
3. Test end-to-end

---

### Scenario 43: Troubleshooting Marathon
**Situation**: Fix 5 different issues in 25 minutes:
1. Node NotReady
2. Pods Pending
3. Service not accessible
4. PVC not binding
5. RBAC permission denied

**Tasks**:
1. Diagnose each issue
2. Fix in priority order
3. Verify all fixes

---

## Advanced Troubleshooting Scenarios

### Scenario 44: ImagePullBackOff Resolution
**Situation**: Multiple pods in ImagePullBackOff state.

**Given**:
- Pods: `app-1`, `app-2`, `app-3`
- All showing ImagePullBackOff
- Image: `private-registry.example.com/app:v1.0`

**Tasks**:
1. Identify the issue (private registry)
2. Create image pull secret
3. Update ServiceAccount or pod spec
4. Verify pods can pull image

**Solution Steps**:
```bash
# Create secret
kubectl create secret docker-registry regcred \
  --docker-server=private-registry.example.com \
  --docker-username=user \
  --docker-password=pass

# Add to ServiceAccount or pod
```

---

### Scenario 45: Readiness Probe Failure
**Situation**: Pods are running but not ready, service has no endpoints.

**Given**:
- Deployment: `web-app`
- Pods: Running but not Ready
- Readiness probe: HTTP on port 80, path `/health`

**Tasks**:
1. Check readiness probe configuration
2. Test probe endpoint manually
3. Fix probe or application
4. Verify pods become ready

---

### Scenario 46: Resource Quota Exceeded
**Situation**: Cannot create new pods, quota exceeded.

**Given**:
- Namespace: `production`
- Error: "exceeded quota: compute-quota"
- Need to create new deployment

**Tasks**:
1. Check ResourceQuota usage
2. Identify exhausted resource
3. Delete unused resources or increase quota
4. Create new deployment

---

### Scenario 47: NetworkPolicy Blocking Legitimate Traffic
**Situation**: After applying NetworkPolicy, pods cannot communicate.

**Given**:
- NetworkPolicy: `web-policy`
- Pods with `app=web` cannot reach `app=api`

**Tasks**:
1. Review NetworkPolicy rules
2. Identify missing rules
3. Update NetworkPolicy
4. Test connectivity

---

### Scenario 48: StatefulSet Pod Deletion and Recreation
**Situation**: Pod `db-statefulset-1` was deleted, verify it recreates correctly.

**Given**:
- StatefulSet: `db-statefulset` with 3 replicas
- Pod `db-statefulset-1` deleted

**Tasks**:
1. Delete the pod
2. Watch recreation
3. Verify pod gets same name
4. Verify PVC is reused
5. Check data persistence

---

### Scenario 49: CronJob Not Creating Jobs
**Situation**: CronJob scheduled but not creating Jobs.

**Given**:
- CronJob: `backup-cronjob`
- Schedule: `0 2 * * *`
- No Jobs created

**Tasks**:
1. Check CronJob status
2. Verify schedule syntax
3. Check if suspended
4. Check controller manager
5. Manually trigger to test

---

### Scenario 50: DaemonSet Update Strategy
**Situation**: Update DaemonSet image without downtime.

**Given**:
- DaemonSet: `log-collector`
- Current image: `fluentd:v1.0`
- New image: `fluentd:v2.0`

**Tasks**:
1. Check current update strategy
2. Update image
3. Monitor rolling update
4. Verify all pods updated

---

## Exam Day Simulation Scenarios

### Full Exam Simulation 1 (2 hours)
Complete these tasks in 2 hours:

1. **Troubleshooting** (30 min)
   - Fix 3 pod issues
   - Fix 2 service issues
   - Fix 1 deployment issue

2. **Deployment** (20 min)
   - Create deployment with 5 replicas
   - Update image
   - Configure probes
   - Set resource limits

3. **Networking** (25 min)
   - Create services (ClusterIP, NodePort)
   - Create Ingress
   - Configure NetworkPolicy

4. **Storage** (20 min)
   - Create PV and PVC
   - Use in StatefulSet
   - Verify persistence

5. **Security** (20 min)
   - Create ServiceAccount
   - Configure RBAC
   - Apply Pod Security Standards

6. **Maintenance** (15 min)
   - Backup resources
   - Drain a node
   - Check cluster health

7. **Advanced** (10 min)
   - Create Job and CronJob
   - Configure DaemonSet
   - Set up ResourceQuota

---

### Full Exam Simulation 2 (2 hours)
Complete these tasks in 2 hours:

1. **Multi-Tier Application** (40 min)
   - Deploy 3-tier app (frontend, backend, database)
   - Configure all networking
   - Set up security

2. **Troubleshooting** (35 min)
   - Fix node issues
   - Fix pod scheduling
   - Fix service discovery
   - Fix storage issues

3. **Configuration** (25 min)
   - ConfigMaps and Secrets
   - ResourceQuotas
   - Pod Disruption Budgets
   - Affinity rules

4. **Advanced Workloads** (20 min)
   - StatefulSet with storage
   - Jobs and CronJobs
   - DaemonSet configuration

---

## Quick Reference: Common Exam Tasks

### 1-Minute Tasks
- Get pod logs
- Describe a resource
- Check service endpoints
- Get events
- Check node status

### 2-Minute Tasks
- Create simple pod
- Create ConfigMap
- Create Secret
- Scale deployment
- Check resource usage

### 3-Minute Tasks
- Update deployment image
- Create Service
- Rollback deployment
- Cordon/uncordon node
- Create Role and RoleBinding

### 5-Minute Tasks
- Fix pod issue
- Create Deployment
- Configure NetworkPolicy
- Create PVC and use in pod
- Set up basic RBAC

### 10-Minute Tasks
- Complete application setup
- Troubleshoot multiple issues
- Configure Ingress
- Set up StatefulSet
- Implement security policies

---

## Practice Schedule Recommendation

### Week 1-2: Basics
- Practice scenarios 1-15 daily
- Focus on basic operations
- Build speed with kubectl

### Week 3-4: Intermediate
- Practice scenarios 16-30
- Focus on troubleshooting
- Time yourself

### Week 5-6: Advanced
- Practice scenarios 31-50
- Full exam simulations
- Identify weak areas

### Week 7-8: Exam Prep
- Daily exam simulations
- Review weak topics
- Practice time management
- Final review

---

## Success Metrics

Track your progress:
- **Speed**: Can you complete basic tasks in < 5 minutes?
- **Accuracy**: Do your solutions work on first try?
- **Troubleshooting**: Can you diagnose issues quickly?
- **Complexity**: Can you handle multi-step scenarios?
- **Time Management**: Can you complete exam simulation in 2 hours?

**Target Metrics**:
- Basic tasks: < 3 minutes each
- Intermediate tasks: < 7 minutes each
- Advanced tasks: < 12 minutes each
- Full exam: Complete in < 2 hours with time to verify

Good luck with your CKA exam preparation! 🚀

