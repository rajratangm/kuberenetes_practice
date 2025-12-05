# Detailed Solutions for Scenario-Based Questions

This document provides complete, step-by-step solutions for all scenario-based questions.

## Basic Scenarios Solutions

### Scenario 1: Simple Pod Deployment Issue

**Problem**: Pod `app-pod` is in Pending state.

**Solution Steps**:

1. **Diagnose the issue**:
```bash
# Check pod status
kubectl get pod app-pod

# Describe pod to see events
kubectl describe pod app-pod

# Check events
kubectl get events --field-selector involvedObject.name=app-pod --sort-by=.lastTimestamp
```

2. **Common fixes based on diagnosis**:

**If insufficient resources**:
```bash
# Check node resources
kubectl describe nodes
kubectl top nodes

# Fix: Reduce resource requests in pod or add more nodes
kubectl edit pod app-pod
# Reduce CPU/memory requests
```

**If node selector mismatch**:
```bash
# Check pod node selector
kubectl get pod app-pod -o yaml | grep nodeSelector

# Fix: Add label to node or remove selector
kubectl label nodes <node-name> <key>=<value>
# OR edit pod to remove nodeSelector
kubectl edit pod app-pod
```

**If PVC not bound**:
```bash
# Check PVC status
kubectl get pvc

# Fix: Create matching PV or fix StorageClass
# See storage scenarios for details
```

3. **Verify fix**:
```bash
kubectl get pod app-pod
kubectl describe pod app-pod
# Should show Running status
```

---

### Scenario 2: Service Not Accessible

**Problem**: Service `web-service` exists but applications cannot connect.

**Solution Steps**:

1. **Check service configuration**:
```bash
# Check service
kubectl get svc web-service -n production
kubectl describe svc web-service -n production

# Check endpoints
kubectl get endpoints web-service -n production
```

2. **Check pod labels**:
```bash
# Get pods with labels
kubectl get pods -n production --show-labels

# Check service selector
kubectl get svc web-service -n production -o yaml | grep -A 5 selector
```

3. **Fix label mismatch** (if needed):
```bash
# Option 1: Update service selector
kubectl edit svc web-service -n production
# Update selector to match pod labels

# Option 2: Update pod labels
kubectl label pods <pod-name> -n production app=web tier=frontend
```

4. **Verify pods are running**:
```bash
kubectl get pods -n production
# Ensure pods are Running, not Pending or CrashLoopBackOff
```

5. **Test connectivity**:
```bash
# Create test pod
kubectl run test-pod --image=busybox:1.35 --rm -it --restart=Never -n production -- wget -O- http://web-service:80

# Or from existing pod
kubectl exec <pod-name> -n production -- wget -O- http://web-service:80
```

6. **Check NetworkPolicy** (if exists):
```bash
kubectl get networkpolicy -n production
# Ensure NetworkPolicy allows traffic
```

**Complete Solution YAML** (if recreating service):
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
  namespace: production
spec:
  selector:
    app: web
    tier: frontend
  ports:
  - port: 80
    targetPort: 80
```

---

### Scenario 3: Deployment Not Scaling

**Problem**: Deployment should have 5 replicas but only shows 3.

**Solution Steps**:

1. **Check deployment status**:
```bash
kubectl get deployment api-deployment
kubectl describe deployment api-deployment
```

2. **Check ReplicaSet**:
```bash
kubectl get rs -l app=api
kubectl describe rs <rs-name>
```

3. **Check pod status**:
```bash
kubectl get pods -l app=api
# Check if pods are Pending
```

4. **Check ResourceQuota**:
```bash
kubectl get resourcequota -n <namespace>
kubectl describe resourcequota <quota-name> -n <namespace>
# Check if quota is exhausted
```

5. **Check node capacity**:
```bash
kubectl get nodes
kubectl describe node <node-name> | grep -A 10 "Allocated resources"
kubectl top nodes
```

6. **Fix based on issue**:

**If ResourceQuota exhausted**:
```bash
# Option 1: Delete unused resources
kubectl get pods --all-namespaces | grep Completed
kubectl delete pod <completed-pod>

# Option 2: Increase quota
kubectl edit resourcequota <quota-name> -n <namespace>
# Increase limits
```

**If node capacity**:
```bash
# Option 1: Add more nodes
# Option 2: Optimize resource requests
kubectl edit deployment api-deployment
# Reduce resource requests
```

**If Pod Disruption Budget**:
```bash
kubectl get pdb
# Check if PDB is preventing scaling
# May need to adjust PDB or deployment strategy
```

7. **Force scaling**:
```bash
kubectl scale deployment api-deployment --replicas=5
kubectl get deployment api-deployment
```

8. **Verify**:
```bash
kubectl get pods -l app=api
# Should show 5 pods
```

---

### Scenario 4: ConfigMap Not Loading

**Problem**: Pod cannot read configuration from ConfigMap.

**Solution Steps**:

1. **Verify ConfigMap exists**:
```bash
kubectl get configmap app-config
kubectl describe configmap app-config
kubectl get configmap app-config -o yaml
```

2. **Check pod configuration**:
```bash
kubectl get pod config-app -o yaml | grep -A 20 volumes
kubectl get pod config-app -o yaml | grep -A 10 volumeMounts
```

3. **Verify ConfigMap keys**:
```bash
kubectl get configmap app-config -o jsonpath='{.data}'
# Should show all keys
```

4. **Fix common issues**:

**If ConfigMap doesn't exist**:
```bash
# Create ConfigMap
kubectl create configmap app-config --from-literal=key1=value1 --from-literal=key2=value2

# Or from file
kubectl create configmap app-config --from-file=config.properties
```

**If volume mount incorrect**:
```yaml
# Fix pod YAML
apiVersion: v1
kind: Pod
metadata:
  name: config-app
spec:
  containers:
  - name: app
    image: busybox:1.35
    command: ['sh', '-c', 'ls -la /etc/config && sleep 3600']
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: app-config  # Must match ConfigMap name
```

**If using environment variables**:
```yaml
spec:
  containers:
  - name: app
    image: busybox:1.35
    env:
    - name: CONFIG_KEY
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: key1
```

5. **Apply fix**:
```bash
kubectl apply -f fixed-pod.yaml
# OR
kubectl delete pod config-app
kubectl create -f fixed-pod.yaml
```

6. **Verify**:
```bash
kubectl exec config-app -- ls /etc/config
kubectl exec config-app -- cat /etc/config/key1
# OR for env vars
kubectl exec config-app -- env | grep CONFIG
```

---

### Scenario 5: Secret Access Denied

**Problem**: Pod needs database credentials but cannot access secret.

**Solution Steps**:

1. **Verify secret exists**:
```bash
kubectl get secret db-credentials
kubectl describe secret db-credentials
```

2. **Check pod configuration**:
```bash
kubectl get pod db-client -o yaml | grep -A 20 env
kubectl get pod db-client -o yaml | grep -A 10 secret
```

3. **Fix secret reference**:

**Using environment variables**:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-client
spec:
  containers:
  - name: app
    image: busybox:1.35
    command: ['sh', '-c', 'env && sleep 3600']
    env:
    - name: DB_USERNAME
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: username
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: password
```

**Using envFrom**:
```yaml
spec:
  containers:
  - name: app
    image: busybox:1.35
    envFrom:
    - secretRef:
        name: db-credentials
```

**Using volume mount**:
```yaml
spec:
  containers:
  - name: app
    image: busybox:1.35
    volumeMounts:
    - name: secret-volume
      mountPath: /etc/secrets
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: db-credentials
```

4. **Create secret if missing**:
```bash
kubectl create secret generic db-credentials \
  --from-literal=username=admin \
  --from-literal=password=secret123
```

5. **Apply and verify**:
```bash
kubectl apply -f fixed-pod.yaml
kubectl exec db-client -- env | grep DB_
# OR
kubectl exec db-client -- cat /etc/secrets/username
```

---

## Intermediate Scenarios Solutions

### Scenario 6: Rolling Update Stuck

**Problem**: Deployment update stuck, not completing rollout.

**Solution Steps**:

1. **Check rollout status**:
```bash
kubectl rollout status deployment/frontend
kubectl rollout history deployment/frontend
```

2. **Check new ReplicaSet**:
```bash
kubectl get rs
kubectl describe rs <new-rs-name>
```

3. **Check pod status in new RS**:
```bash
kubectl get pods -l app=frontend
kubectl describe pod <pod-name>
```

4. **Check events**:
```bash
kubectl get events --sort-by=.lastTimestamp | grep frontend
```

5. **Common fixes**:

**If ImagePullBackOff**:
```bash
# Check image name
kubectl get deployment frontend -o yaml | grep image

# Fix: Update to correct image
kubectl set image deployment/frontend nginx=nginx:1.22

# Or if private registry, add image pull secret
kubectl create secret docker-registry regcred \
  --docker-server=registry.example.com \
  --docker-username=user \
  --docker-password=pass

# Add to deployment
kubectl patch deployment frontend -p '{"spec":{"template":{"spec":{"imagePullSecrets":[{"name":"regcred"}]}}}}'
```

**If Readiness probe failing**:
```bash
# Check probe configuration
kubectl get deployment frontend -o yaml | grep -A 10 readinessProbe

# Test probe manually
kubectl exec <pod-name> -- curl localhost:80/health

# Fix: Update probe or application
kubectl edit deployment frontend
# Adjust readinessProbe path or timing
```

**If resource limits too low**:
```bash
# Check resource usage
kubectl top pod <pod-name>

# Increase limits
kubectl edit deployment frontend
# Increase resources.limits
```

6. **Rollback if needed**:
```bash
kubectl rollout undo deployment/frontend
kubectl rollout status deployment/frontend
```

7. **Verify fix**:
```bash
kubectl get deployment frontend
kubectl get pods -l app=frontend
# All pods should be Running and Ready
```

---

### Scenario 7: Persistent Volume Not Binding

**Problem**: PVC stuck in Pending state.

**Solution Steps**:

1. **Check PVC details**:
```bash
kubectl get pvc data-pvc
kubectl describe pvc data-pvc
# Look for events and conditions
```

2. **Check available PVs**:
```bash
kubectl get pv
kubectl describe pv
# Check which PVs are Available
```

3. **Check StorageClass**:
```bash
kubectl get storageclass fast-ssd
kubectl describe storageclass fast-ssd
```

4. **Fix based on issue**:

**If no matching PV**:
```yaml
# Create matching PV
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-manual
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce  # Must match PVC
  persistentVolumeReclaimPolicy: Retain
  storageClassName: fast-ssd  # Must match PVC
  hostPath:
    path: /mnt/data
```

```bash
kubectl apply -f pv-manual.yaml
kubectl get pvc data-pvc
# Should bind automatically
```

**If StorageClass issue**:
```bash
# Check if StorageClass exists
kubectl get storageclass

# Create StorageClass if missing
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```

**If access mode mismatch**:
```bash
# Check PVC access mode
kubectl get pvc data-pvc -o yaml | grep accessModes

# Check PV access modes
kubectl get pv -o yaml | grep accessModes

# Fix: Update PVC or create matching PV
```

5. **Verify binding**:
```bash
kubectl get pvc data-pvc
# Status should be Bound
kubectl get pv
# PV should show Bound to data-pvc
```

---

### Scenario 8: NetworkPolicy Blocking Traffic

**Problem**: Pods cannot communicate after NetworkPolicy applied.

**Solution Steps**:

1. **Review NetworkPolicy**:
```bash
kubectl get networkpolicy web-policy -n production
kubectl describe networkpolicy web-policy -n production
kubectl get networkpolicy web-policy -n production -o yaml
```

2. **Check pod labels**:
```bash
kubectl get pods -n production --show-labels
# Verify labels match NetworkPolicy selectors
```

3. **Test connectivity**:
```bash
# From web pod to api pod
kubectl exec <web-pod> -n production -- wget -O- http://<api-pod-ip>:8080
# Should fail if blocked
```

4. **Fix NetworkPolicy**:

**Allow web to api**:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: web-policy
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: web
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 80
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: api
    ports:
    - protocol: TCP
      port: 8080
```

**Allow api to receive from web**:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: api-policy
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: api
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: web
    ports:
    - protocol: TCP
      port: 8080
```

5. **Apply fix**:
```bash
kubectl apply -f fixed-networkpolicy.yaml
```

6. **Verify connectivity**:
```bash
kubectl exec <web-pod> -n production -- wget -O- http://api-service:8080
# Should succeed now
```

---

### Scenario 9: RBAC Permission Denied

**Problem**: ServiceAccount cannot list pods.

**Solution Steps**:

1. **Check ServiceAccount**:
```bash
kubectl get serviceaccount api-sa -n development
kubectl describe serviceaccount api-sa -n development
```

2. **Test current permissions**:
```bash
kubectl auth can-i list pods --as=system:serviceaccount:development:api-sa -n development
kubectl auth can-i --list --as=system:serviceaccount:development:api-sa -n development
```

3. **Check Role and RoleBinding**:
```bash
kubectl get role,rolebinding -n development
kubectl describe role <role-name> -n development
kubectl describe rolebinding <binding-name> -n development
```

4. **Fix RBAC**:

**Create/Update Role**:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: development
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

**Create/Update RoleBinding**:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader-binding
  namespace: development
subjects:
- kind: ServiceAccount
  name: api-sa
  namespace: development
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

5. **Apply**:
```bash
kubectl apply -f role.yaml
kubectl apply -f rolebinding.yaml
```

6. **Verify**:
```bash
kubectl auth can-i list pods --as=system:serviceaccount:development:api-sa -n development
# Should return yes
```

---

### Scenario 10: Ingress Not Routing

**Problem**: Ingress exists but traffic not reaching backend.

**Solution Steps**:

1. **Check Ingress**:
```bash
kubectl get ingress web-ingress
kubectl describe ingress web-ingress
kubectl get ingress web-ingress -o yaml
```

2. **Check Ingress controller**:
```bash
kubectl get pods -n ingress-nginx  # or kube-system
kubectl get pods -l app.kubernetes.io/component=controller
# Verify controller is running
```

3. **Check backend service**:
```bash
kubectl get svc web-service
kubectl get endpoints web-service
# Verify service has endpoints
```

4. **Check Ingress status**:
```bash
kubectl get ingress web-ingress -o jsonpath='{.status.loadBalancer.ingress}'
# Should show IP or hostname
```

5. **Fix common issues**:

**If controller not running**:
```bash
# Install or restart controller
# Depends on your setup (nginx, traefik, etc.)
```

**If service selector mismatch**:
```bash
# Fix service selector
kubectl edit svc web-service
# Ensure selector matches pod labels
```

**If Ingress configuration wrong**:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web-service
            port:
              number: 80
```

6. **Test routing**:
```bash
# Add to /etc/hosts or use Host header
curl -H "Host: app.example.com" http://<ingress-ip>/

# Or from pod
kubectl run test --image=curlimages/curl --rm -it --restart=Never -- curl -H "Host: app.example.com" http://<ingress-ip>/
```

---

## Advanced Scenarios Solutions

### Scenario 13: Multi-Namespace Service Discovery

**Problem**: Pod in production namespace cannot access service in development.

**Solution Steps**:

1. **Verify service exists**:
```bash
kubectl get svc dev-api -n development
kubectl get endpoints dev-api -n development
```

2. **Check source pod**:
```bash
kubectl get pod prod-app -n production
```

3. **Use FQDN**:
```bash
# Test from production pod
kubectl exec prod-app -n production -- nslookup dev-api.development.svc.cluster.local

# Test connectivity
kubectl exec prod-app -n production -- wget -O- http://dev-api.development.svc.cluster.local:8080
```

4. **Check NetworkPolicy**:
```bash
kubectl get networkpolicy -n development
kubectl get networkpolicy -n production

# Allow cross-namespace traffic if needed
```

5. **Fix NetworkPolicy if blocking**:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-production
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: api
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: production
    ports:
    - protocol: TCP
      port: 8080
```

6. **Verify**:
```bash
kubectl exec prod-app -n production -- wget -O- http://dev-api.development.svc.cluster.local:8080
# Should succeed
```

---

### Scenario 14: Cluster Resource Exhaustion

**Problem**: New pods cannot be scheduled.

**Solution Steps**:

1. **Check node resources**:
```bash
kubectl top nodes
kubectl describe nodes | grep -A 10 "Allocated resources"
```

2. **Check pending pods**:
```bash
kubectl get pods --all-namespaces --field-selector=status.phase=Pending
kubectl describe pod <pending-pod>
```

3. **Identify resource-hungry pods**:
```bash
kubectl top pods --all-namespaces --sort-by=cpu
kubectl top pods --all-namespaces --sort-by=memory
```

4. **Check ResourceQuotas**:
```bash
kubectl get resourcequota --all-namespaces
kubectl describe resourcequota <quota-name> -n <namespace>
```

5. **Fix options**:

**Optimize resource requests**:
```bash
# Find pods with high requests
kubectl get pods -o json | jq '.items[] | {name: .metadata.name, requests: .spec.containers[].resources.requests}'

# Edit deployment to reduce requests
kubectl edit deployment <name>
# Reduce CPU/memory requests to match actual usage
```

**Delete unused resources**:
```bash
# Delete completed jobs
kubectl get jobs --all-namespaces | grep Complete
kubectl delete job <job-name> -n <namespace>

# Delete evicted pods
kubectl get pods --all-namespaces | grep Evicted
kubectl delete pod <pod-name> -n <namespace>
```

**Increase ResourceQuota**:
```bash
kubectl edit resourcequota <quota-name> -n <namespace>
# Increase limits
```

6. **Verify**:
```bash
kubectl get pods --field-selector=status.phase=Pending
# Should be fewer or none
```

---

### Scenario 15: etcd Backup and Restore

**Problem**: Need to backup etcd and understand restore.

**Solution Steps**:

1. **Find etcd pod**:
```bash
kubectl get pods -n kube-system | grep etcd
kubectl describe pod <etcd-pod> -n kube-system
```

2. **Get etcd endpoints and certificates**:
```bash
# Usually in /etc/kubernetes/pki/etcd/ on control plane node
# Or check etcd pod volume mounts
kubectl get pod <etcd-pod> -n kube-system -o yaml | grep -A 10 volumeMounts
```

3. **Create backup**:
```bash
# On control plane node
ETCDCTL_API=3 etcdctl snapshot save /backup/etcd-snapshot-$(date +%Y%m%d).db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key
```

4. **Verify backup**:
```bash
ETCDCTL_API=3 etcdctl snapshot status /backup/etcd-snapshot-*.db
ls -lh /backup/etcd-snapshot-*.db
```

5. **Restore procedure** (documentation):
```bash
# 1. Stop kube-apiserver
# 2. Restore from snapshot
ETCDCTL_API=3 etcdctl snapshot restore /backup/etcd-snapshot.db \
  --data-dir=/var/lib/etcd-backup

# 3. Update etcd manifest to use new data-dir
# 4. Restart etcd
# 5. Restart kube-apiserver
# 6. Verify cluster health
```

6. **Verify cluster health**:
```bash
kubectl get nodes
kubectl get pods --all-namespaces
```

---

### Scenario 16: Node Maintenance with Zero Downtime

**Problem**: Perform maintenance on node without affecting workloads.

**Solution Steps**:

1. **Cordon the node**:
```bash
kubectl cordon worker-node-1
kubectl get nodes worker-node-1
# Should show SchedulingDisabled
```

2. **Drain the node**:
```bash
kubectl drain worker-node-1 \
  --ignore-daemonsets \
  --delete-emptydir-data \
  --timeout=300s
```

3. **Monitor pod rescheduling**:
```bash
kubectl get pods --all-namespaces -o wide | grep worker-node-1
# Pods should move to other nodes
kubectl get pods --all-namespaces -o wide | grep -v worker-node-1
# Verify pods running on other nodes
```

4. **Perform maintenance** (simulated):
```bash
# Example: Update system packages, reboot, etc.
# This is simulated in exam
```

5. **Uncordon the node**:
```bash
kubectl uncordon worker-node-1
kubectl get nodes worker-node-1
# Should show Ready and schedulable
```

6. **Verify**:
```bash
kubectl get nodes
kubectl get pods --all-namespaces
# All pods should be Running
```

---

### Scenario 17: Deployment Rollback Under Pressure

**Problem**: Recent update causing issues, need immediate rollback.

**Solution Steps**:

1. **Check rollout history**:
```bash
kubectl rollout history deployment/critical-app
kubectl rollout history deployment/critical-app --revision=2
```

2. **Identify previous working revision**:
```bash
# Usually revision 1 or 2
kubectl rollout history deployment/critical-app | head -5
```

3. **Perform rollback**:
```bash
# Rollback to previous version
kubectl rollout undo deployment/critical-app

# OR rollback to specific revision
kubectl rollout undo deployment/critical-app --to-revision=1
```

4. **Monitor rollback**:
```bash
kubectl rollout status deployment/critical-app
kubectl get pods -l app=critical-app -w
```

5. **Verify rollback success**:
```bash
kubectl get deployment critical-app
kubectl get pods -l app=critical-app
# Check pod logs
kubectl logs <pod-name> -l app=critical-app
```

---

## Real Exam-Style Quick Solutions

### Scenario 31: Pod CrashLoopBackOff - Quick Fix

**Solution**:
```bash
# 1. Get logs
kubectl logs api-server --previous

# 2. Common fixes:
# If wrong command - edit pod/deployment
kubectl edit deployment api-server
# Fix command or image

# If missing env var
kubectl set env deployment/api-server KEY=value

# If resource limits
kubectl edit deployment api-server
# Increase limits

# 3. Verify
kubectl get pod api-server
```

---

### Scenario 32: Service Endpoint Missing - Quick Fix

**Solution**:
```bash
# 1. Check selector
kubectl get svc web-svc -o yaml | grep selector

# 2. Check pod labels
kubectl get pods --show-labels

# 3. Fix mismatch
kubectl label pods <pod-name> app=web
# OR
kubectl edit svc web-svc
# Update selector

# 4. Verify
kubectl get endpoints web-svc
```

---

### Scenario 33: Deployment Image Update

**Solution**:
```bash
# Update image
kubectl set image deployment/frontend nginx=nginx:1.23

# Monitor
kubectl rollout status deployment/frontend

# Verify
kubectl get pods -l app=frontend
kubectl describe pod <pod-name> | grep Image
```

---

### Scenario 34: Create ConfigMap and Use in Pod

**Solution**:
```bash
# 1. Create ConfigMap
kubectl create configmap app-config --from-literal=env=production

# 2. Create pod
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app
    image: busybox:1.35
    command: ['sh', '-c', 'env && sleep 3600']
    env:
    - name: ENV
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: env
EOF

# 3. Verify
kubectl exec app-pod -- env | grep ENV
```

---

### Scenario 35: RBAC - Allow Pod Creation

**Solution**:
```bash
# 1. Create Role
kubectl create role pod-creator \
  --verb=create \
  --resource=pods \
  -n development

# 2. Create RoleBinding
kubectl create rolebinding pod-creator-binding \
  --role=pod-creator \
  --serviceaccount=development:dev-sa \
  -n development

# 3. Test
kubectl auth can-i create pods \
  --as=system:serviceaccount:development:dev-sa \
  -n development
```

---

## Complete YAML Solutions Repository

All YAML solutions are available in the `solutions/` directories of each scenario folder. For quick reference, common patterns are in `SCENARIO_SOLUTIONS.md`.

## Practice Tips

1. **Time yourself**: Each scenario has a time limit - practice meeting it
2. **Verify everything**: Always test your solutions
3. **Use explain**: `kubectl explain` when unsure
4. **Check events**: `kubectl get events` shows what's happening
5. **Read carefully**: Misreading requirements wastes time

---

## Additional Scenario Solutions

### Scenario 11: StatefulSet Pod Not Starting

**Solution**:
```bash
# 1. Check pod status
kubectl get pod db-statefulset-1
kubectl describe pod db-statefulset-1

# 2. Check PVC for this pod
kubectl get pvc | grep db-statefulset-1

# 3. Common fixes:
# If PVC not bound - check StorageClass
kubectl get storageclass
kubectl describe pvc data-db-statefulset-1

# If node issues - check node capacity
kubectl describe node <node-name>

# 4. Fix and verify
kubectl get pod db-statefulset-1
# Should be Running
```

---

### Scenario 12: Job Not Completing

**Solution**:
```bash
# 1. Check job status
kubectl get job data-processor
kubectl describe job data-processor

# 2. Check pod logs
kubectl logs job/data-processor
kubectl get pods -l job-name=data-processor

# 3. Common fixes:
# If command hanging - check logs and fix command
kubectl logs <pod-name>

# If resource limits - increase
kubectl edit job data-processor
# Increase resources

# If missing dependencies - add init containers or fix command

# 4. Delete and recreate with fix
kubectl delete job data-processor
kubectl apply -f fixed-job.yaml
```

---

### Scenario 18: Pod Security Policy Violation

**Solution**:
```yaml
# Pod with proper security context for restricted namespace
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
  namespace: restricted-ns
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
    runAsGroup: 2000
    fsGroup: 2000
  containers:
  - name: app
    image: nginx:1.21
    securityContext:
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false
      capabilities:
        drop:
        - ALL
        add:
        - NET_BIND_SERVICE
    volumeMounts:
    - name: tmp
      mountPath: /tmp
    - name: var-cache
      mountPath: /var/cache/nginx
  volumes:
  - name: tmp
    emptyDir: {}
  - name: var-cache
    emptyDir: {}
```

---

### Scenario 19: Service Mesh-like Configuration

**Solution**:
```yaml
# NetworkPolicy for Frontend
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: frontend-policy
spec:
  podSelector:
    matchLabels:
      app: frontend
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - ports:
    - protocol: TCP
      port: 80
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: backend
    ports:
    - protocol: TCP
      port: 8080
---
# NetworkPolicy for Backend
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend-policy
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 8080
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: database
    ports:
    - protocol: TCP
      port: 3306
---
# NetworkPolicy for Database
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: database-policy
spec:
  podSelector:
    matchLabels:
      app: database
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: backend
    ports:
    - protocol: TCP
      port: 3306
```

---

### Scenario 20: Multi-Container Pod Coordination

**Solution**:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-sidecar
spec:
  initContainers:
  - name: wait-for-app
    image: busybox:1.35
    command: ['sh', '-c', 'until nc -z localhost 8080; do echo waiting for app; sleep 2; done']
  containers:
  - name: app
    image: nginx:1.21
    ports:
    - containerPort: 8080
    volumeMounts:
    - name: shared
      mountPath: /shared
  - name: sidecar
    image: busybox:1.35
    command: ['sh', '-c', 'tail -f /shared/app.log']
    volumeMounts:
    - name: shared
      mountPath: /shared
  volumes:
  - name: shared
    emptyDir: {}
```

---

### Scenario 21: Resource Quota Exceeded

**Solution**:
```bash
# 1. Check quota
kubectl get resourcequota -n production
kubectl describe resourcequota compute-quota -n production

# 2. Check usage
kubectl get pods -n production
kubectl top pods -n production

# 3. Fix options:
# Option A: Delete unused resources
kubectl get pods -n production | grep Completed
kubectl delete pod <completed-pod> -n production

# Option B: Increase quota
kubectl edit resourcequota compute-quota -n production
# Increase limits

# Option C: Optimize existing pods
kubectl get deployment -n production
kubectl edit deployment <name> -n production
# Reduce resource requests

# 4. Verify
kubectl get resourcequota compute-quota -n production
# Check used vs hard limits
```

---

### Scenario 22: CronJob Not Running

**Solution**:
```bash
# 1. Check CronJob
kubectl get cronjob backup-job
kubectl describe cronjob backup-job

# 2. Check if suspended
kubectl get cronjob backup-job -o yaml | grep suspend

# 3. Check schedule syntax
kubectl get cronjob backup-job -o yaml | grep schedule

# 4. Fix if suspended
kubectl patch cronjob backup-job -p '{"spec":{"suspend":false}}'

# 5. Manually trigger to test
kubectl create job --from=cronjob/backup-job backup-manual-$(date +%s)

# 6. Check jobs created
kubectl get jobs -l app=backup
```

---

### Scenario 23: DaemonSet Not Running on All Nodes

**Solution**:
```bash
# 1. Check DaemonSet status
kubectl get daemonset log-collector
kubectl describe daemonset log-collector

# 2. Check nodes
kubectl get nodes
kubectl get nodes --show-labels

# 3. Check DaemonSet node selector
kubectl get daemonset log-collector -o yaml | grep nodeSelector

# 4. Check node taints
kubectl describe node <node-name> | grep Taints

# 5. Fix:
# If node selector too restrictive - remove or update
kubectl edit daemonset log-collector
# Remove or update nodeSelector

# If taints - add tolerations
kubectl edit daemonset log-collector
# Add tolerations section

# 6. Verify
kubectl get pods -l app=log-collector -o wide
# Should see one pod per node
```

---

### Scenario 24: StatefulSet Scaling Issues

**Solution**:
```bash
# 1. Check StatefulSet
kubectl get statefulset db-statefulset
kubectl describe statefulset db-statefulset

# 2. Check PVCs
kubectl get pvc | grep db-statefulset

# 3. Check StorageClass
kubectl get storageclass
kubectl describe storageclass <sc-name>

# 4. Fix:
# If StorageClass missing - create it
kubectl apply -f storageclass.yaml

# If storage exhausted - check cluster storage
kubectl get pv

# 5. Scale
kubectl scale statefulset db-statefulset --replicas=5

# 6. Monitor
kubectl get pods -l app=db -w
kubectl get pvc -w
```

---

### Scenario 25: Pod Disruption Budget Preventing Updates

**Solution**:
```bash
# 1. Check PDB
kubectl get pdb web-pdb
kubectl describe pdb web-pdb

# 2. Check deployment
kubectl get deployment web-app
kubectl describe deployment web-app

# 3. Check deployment strategy
kubectl get deployment web-app -o yaml | grep -A 5 strategy

# 4. Fix:
# Option A: Temporarily adjust PDB
kubectl edit pdb web-pdb
# Reduce minAvailable temporarily

# Option B: Adjust deployment strategy
kubectl edit deployment web-app
# Increase maxUnavailable to allow more pods down during update

# 5. Update deployment
kubectl set image deployment/web-app nginx=nginx:1.23

# 6. Restore PDB after update
kubectl edit pdb web-pdb
# Restore original minAvailable
```

---

## Quick Command Reference for All Scenarios

### Diagnostic Commands
```bash
# Pods
kubectl get pods -o wide
kubectl describe pod <name>
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl get events --field-selector involvedObject.name=<pod-name>

# Services
kubectl get svc
kubectl get endpoints
kubectl describe svc <name>

# Deployments
kubectl get deployment
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>

# Storage
kubectl get pv,pvc
kubectl describe pvc <name>
kubectl get storageclass

# Nodes
kubectl get nodes
kubectl describe node <name>
kubectl top nodes

# RBAC
kubectl get role,rolebinding
kubectl auth can-i <verb> <resource> --as=system:serviceaccount:<ns>:<sa>
```

### Quick Fix Commands
```bash
# Update image
kubectl set image deployment/<name> <container>=<image>

# Scale
kubectl scale deployment <name> --replicas=<n>

# Rollback
kubectl rollout undo deployment/<name>

# Cordon/Uncordon
kubectl cordon <node>
kubectl uncordon <node>

# Drain
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data

# Label
kubectl label pods <name> <key>=<value>
kubectl label nodes <name> <key>=<value>

# Taint
kubectl taint nodes <name> <key>=<value>:<effect>
kubectl taint nodes <name> <key>=<value>:<effect>-
```

## Solution Files Location

All YAML solution files are in:
- `scenario-solutions/` - Quick reference YAML files
- Individual scenario folders `solutions/` - Detailed solutions
- This document - Complete step-by-step solutions

## Practice Strategy

1. **Read scenario** - Understand the problem
2. **Try yourself** - Attempt solution without looking
3. **Check solution** - Compare your approach
4. **Practice again** - Do it multiple times
5. **Time yourself** - Meet the time targets
6. **Verify always** - Test your solutions work

Good luck with your CKA exam! 🚀

