# Level 2: Intermediate - Practical Skills

Build practical Kubernetes administration skills with real-world scenarios.

## Topic 1: Advanced Pod Configuration

### Question 2.1: Pod with Resource Limits
**Task**: Create a pod with resource requests and limits.

**Steps**:
1. Create a pod with CPU request 100m and limit 200m
2. Create a pod with Memory request 128Mi and limit 256Mi
3. Create a pod with both CPU and Memory limits
4. Verify resource allocation
5. Understand the difference between requests and limits

**Solution**:
See `solutions/pod-resources.yaml`

**Expected Time**: 5 minutes

---

### Question 2.2: Pod with Health Probes
**Task**: Configure health probes for pods.

**Steps**:
1. Create a pod with readiness probe
2. Create a pod with liveness probe
3. Create a pod with startup probe
4. Understand when each probe runs
5. Test probe behavior

**Solution**:
See `solutions/pod-probes.yaml`

**Expected Time**: 6 minutes

---

### Question 2.3: Multi-Container Pod
**Task**: Create a pod with multiple containers.

**Steps**:
1. Create a pod with 2 containers sharing a volume
2. Container 1 writes data, Container 2 reads it
3. Verify data sharing works
4. Check logs from both containers
5. Understand use cases

**Solution**:
See `solutions/pod-multi-container.yaml`

**Expected Time**: 6 minutes

---

### Question 2.4: Pod with Init Containers
**Task**: Use init containers for setup tasks.

**Steps**:
1. Create a pod with init container that waits for service
2. Create a pod with init container that downloads config
3. Verify init containers run before main container
4. Check init container logs
5. Understand execution order

**Solution**:
See `solutions/pod-init-containers.yaml`

**Expected Time**: 6 minutes

---

## Topic 2: Advanced Deployments

### Question 2.5: Deployment Strategies
**Task**: Understand and configure deployment strategies.

**Steps**:
1. Create deployment with RollingUpdate strategy
2. Configure maxSurge and maxUnavailable
3. Test rolling update behavior
4. Create deployment with Recreate strategy
5. Compare both strategies

**Solution**:
See `solutions/deployment-strategies.yaml`

**Expected Time**: 7 minutes

---

### Question 2.6: Deployment with Environment Variables
**Task**: Configure deployments with environment variables.

**Steps**:
1. Create deployment with env vars from ConfigMap
2. Create deployment with env vars from Secret
3. Use envFrom for multiple keys
4. Update ConfigMap and restart pods
5. Verify environment variables

**Solution**:
See `solutions/deployment-env.yaml`

**Expected Time**: 7 minutes

---

### Question 2.7: Deployment Rollout Management
**Task**: Master deployment rollouts.

**Steps**:
1. Create deployment and make multiple updates
2. Check rollout history with reasons
3. Pause a rollout
4. Resume a rollout
5. Rollback to specific revision
6. Understand revision limits

**Solution**:
```bash
# Create and update
kubectl create deployment app --image=nginx:1.21
kubectl set image deployment/app nginx=nginx:1.22
kubectl set image deployment/app nginx=nginx:1.23

# History with reasons
kubectl rollout history deployment/app --revision=2

# Pause
kubectl rollout pause deployment/app
kubectl set image deployment/app nginx=nginx:1.24

# Resume
kubectl rollout resume deployment/app

# Rollback
kubectl rollout undo deployment/app --to-revision=1
```

**Expected Time**: 8 minutes

---

## Topic 3: Services Deep Dive

### Question 2.8: Service Endpoints
**Task**: Understand and work with service endpoints.

**Steps**:
1. Create a service and check endpoints
2. Manually create endpoints for external service
3. Update service selector and observe endpoints
4. Check endpoint subsets
5. Understand endpoint controller

**Solution**:
See `solutions/service-endpoints.yaml`

**Expected Time**: 6 minutes

---

### Question 2.9: Headless Services
**Task**: Create and use headless services.

**Steps**:
1. Create a headless service (ClusterIP: None)
2. Create pods that the service selects
3. Test DNS resolution (returns all pod IPs)
4. Understand use cases (StatefulSets, service discovery)
5. Compare with regular service

**Solution**:
See `solutions/service-headless.yaml`

**Expected Time**: 6 minutes

---

### Question 2.10: Service Session Affinity
**Task**: Configure session affinity.

**Steps**:
1. Create service with session affinity
2. Configure session affinity timeout
3. Test session affinity behavior
4. Understand ClientIP vs None
5. Use cases for session affinity

**Solution**:
See `solutions/service-session-affinity.yaml`

**Expected Time**: 5 minutes

---

## Topic 4: ConfigMaps and Secrets Advanced

### Question 2.11: ConfigMap Hot Reload
**Task**: Understand ConfigMap updates.

**Steps**:
1. Create ConfigMap and pod using it as volume
2. Update ConfigMap
3. Check if pod picks up changes
4. Understand sync period
5. Restart pod to get new config

**Solution**:
```bash
# Create ConfigMap
kubectl create configmap app-config --from-literal=key=value1

# Create pod (see solutions/configmap-pod-volume.yaml)
kubectl apply -f solutions/configmap-pod-volume.yaml

# Update ConfigMap
kubectl create configmap app-config --from-literal=key=value2 --dry-run=client -o yaml | kubectl apply -f -

# Check (may need to wait for sync period or restart pod)
kubectl exec configmap-volume-pod -- cat /etc/config/key

# Restart to force reload
kubectl delete pod configmap-volume-pod
kubectl apply -f solutions/configmap-pod-volume.yaml
```

**Expected Time**: 7 minutes

---

### Question 2.12: Secret Types
**Task**: Work with different secret types.

**Steps**:
1. Create generic secret
2. Create docker-registry secret
3. Create TLS secret
4. Use each type appropriately
5. Understand when to use each

**Solution**:
```bash
# Generic
kubectl create secret generic app-secret --from-literal=key=value

# Docker registry
kubectl create secret docker-registry regcred \
  --docker-server=registry.example.com \
  --docker-username=user \
  --docker-password=pass

# TLS
kubectl create secret tls tls-secret --cert=tls.crt --key=tls.key
```

**Expected Time**: 6 minutes

---

## Topic 5: Storage Basics

### Question 2.13: PersistentVolume and PersistentVolumeClaim
**Task**: Create and use persistent storage.

**Steps**:
1. Create a PersistentVolume (5Gi, ReadWriteOnce)
2. Create a PersistentVolumeClaim (3Gi)
3. Verify PVC binds to PV
4. Create a pod using the PVC
5. Write data and verify persistence

**Solution**:
See `solutions/pv-pvc-pod.yaml`

**Expected Time**: 8 minutes

---

### Question 2.14: StorageClass
**Task**: Create and use StorageClass.

**Steps**:
1. Create a StorageClass
2. Create a PVC using the StorageClass
3. Verify dynamic provisioning (if supported)
4. Set StorageClass as default
5. Understand provisioners

**Solution**:
See `solutions/storageclass-pvc.yaml`

**Expected Time**: 7 minutes

---

## Topic 6: Basic RBAC

### Question 2.15: Create Role and RoleBinding
**Task**: Set up basic RBAC.

**Steps**:
1. Create a ServiceAccount
2. Create a Role with pod permissions
3. Create a RoleBinding
4. Test permissions
5. Verify access works

**Solution**:
See `solutions/rbac-basic.yaml`

**Expected Time**: 7 minutes

---

### Question 2.16: Check Permissions
**Task**: Verify RBAC permissions.

**Steps**:
1. Use `kubectl auth can-i` to check permissions
2. List all permissions for a ServiceAccount
3. Test different verbs and resources
4. Understand permission evaluation
5. Troubleshoot permission issues

**Solution**:
```bash
# Check single permission
kubectl auth can-i create pods
kubectl auth can-i create pods --as=system:serviceaccount:default:my-sa

# List all permissions
kubectl auth can-i --list
kubectl auth can-i --list --as=system:serviceaccount:default:my-sa

# Test in namespace
kubectl auth can-i create pods -n production
```

**Expected Time**: 5 minutes

---

## Topic 7: Basic Troubleshooting

### Question 2.17: Diagnose Pod Issues
**Task**: Troubleshoot common pod problems.

**Steps**:
1. Create a pod that will be Pending (resource issue)
2. Diagnose why it's pending
3. Create a pod that will CrashLoopBackOff
4. Diagnose the crash
5. Fix both issues

**Solution**:
```bash
# Pending pod (resource issue)
kubectl run pending-pod --image=nginx:1.21 --requests=cpu=1000,memory=1Ti
kubectl describe pod pending-pod
# Fix: Reduce resource requests

# CrashLoopBackOff
kubectl run crash-pod --image=nginx:1.21 -- command=exit 1
kubectl logs crash-pod --previous
kubectl describe pod crash-pod
# Fix: Correct command or image
```

**Expected Time**: 10 minutes

---

### Question 2.18: Diagnose Service Issues
**Task**: Troubleshoot service connectivity.

**Steps**:
1. Create a service with no endpoints
2. Diagnose why no endpoints
3. Fix the selector mismatch
4. Test connectivity
5. Verify endpoints are created

**Solution**:
```bash
# Create pods
kubectl run pod1 --image=nginx:1.21 --labels="app=web"

# Create service with wrong selector
kubectl create service clusterip web-svc --tcp=80:80
kubectl patch svc web-svc -p '{"spec":{"selector":{"app":"api"}}}'

# Check endpoints (empty)
kubectl get endpoints web-svc

# Fix selector
kubectl patch svc web-svc -p '{"spec":{"selector":{"app":"web"}}}'

# Verify
kubectl get endpoints web-svc
```

**Expected Time**: 8 minutes

---

## Practice Exercises

### Exercise 1: Complete Application
Deploy a complete application:
1. Namespace: `intermediate-practice`
2. Deployment: 3 replicas, with health probes, resource limits
3. Service: ClusterIP exposing the deployment
4. ConfigMap: Application configuration
5. Secret: Database credentials
6. RBAC: ServiceAccount with pod read permissions
7. Storage: PVC mounted in deployment

**Expected Time**: 20 minutes

### Exercise 2: Troubleshooting Challenge
Fix multiple issues:
1. Deployment not updating
2. Service has no endpoints
3. Pod cannot access ConfigMap
4. RBAC permission denied
5. PVC not binding

**Expected Time**: 25 minutes

---

## Solutions

All solutions are in the `solutions/` folder.

## Next Level

Once comfortable with intermediate level, proceed to:
- **03-advanced/** - Complex scenarios and expert techniques

