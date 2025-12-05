# Scenario-Based Questions - Quick Solutions Guide

This document provides quick solution approaches for common CKA exam scenarios.

## Quick Diagnostic Commands

### Pod Issues
```bash
# Check pod status
kubectl get pod <pod-name> -o wide

# Describe pod (shows events and conditions)
kubectl describe pod <pod-name>

# Get pod logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous

# Check events
kubectl get events --field-selector involvedObject.name=<pod-name> --sort-by=.lastTimestamp
```

### Service Issues
```bash
# Check service
kubectl get svc <service-name> -o yaml

# Check endpoints
kubectl get endpoints <service-name>

# Describe service
kubectl describe svc <service-name>

# Test connectivity
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- wget -O- http://<service-name>
```

### Deployment Issues
```bash
# Check deployment status
kubectl get deployment <name>
kubectl rollout status deployment/<name>

# Check rollout history
kubectl rollout history deployment/<name>

# Describe deployment
kubectl describe deployment <name>

# Check ReplicaSet
kubectl get rs -l app=<label>
```

### Storage Issues
```bash
# Check PVC status
kubectl get pvc
kubectl describe pvc <pvc-name>

# Check PV status
kubectl get pv
kubectl describe pv <pv-name>

# Check StorageClass
kubectl get storageclass
```

### Node Issues
```bash
# Check node status
kubectl get nodes
kubectl describe node <node-name>

# Check node resources
kubectl top nodes
kubectl describe node <node-name> | grep -A 10 "Allocated resources"
```

## Common Fix Patterns

### Pattern 1: Pod Pending
**Diagnosis**:
1. `kubectl describe pod <name>` - Check events
2. Common causes:
   - Insufficient resources
   - Node selector mismatch
   - Taints without tolerations
   - PVC not bound

**Quick Fixes**:
- Resource issue: Check node capacity, reduce requests
- Selector: Add label to node or remove selector
- Taints: Add toleration or remove taint
- PVC: Create matching PV or fix StorageClass

### Pattern 2: Pod CrashLoopBackOff
**Diagnosis**:
1. `kubectl logs <pod-name> --previous`
2. `kubectl describe pod <pod-name>`
3. Common causes:
   - Application error
   - Wrong command
   - Missing env var
   - Resource limits too low

**Quick Fixes**:
- Check logs for error message
- Fix command or image
- Add missing environment variables
- Increase resource limits

### Pattern 3: Service No Endpoints
**Diagnosis**:
1. `kubectl get endpoints <service-name>`
2. `kubectl get pods --show-labels`
3. `kubectl get svc <service-name> -o yaml | grep selector`

**Quick Fix**:
- Ensure service selector matches pod labels exactly
- Verify pods are running (not Pending/CrashLoopBackOff)

### Pattern 4: Deployment Rollout Stuck
**Diagnosis**:
1. `kubectl rollout status deployment/<name>`
2. `kubectl get rs`
3. `kubectl describe deployment <name>`

**Quick Fixes**:
- ImagePullBackOff: Fix image name/tag or add pull secret
- Readiness probe failing: Fix probe or application
- Resource limits: Increase limits or check quota
- Rollback: `kubectl rollout undo deployment/<name>`

### Pattern 5: PVC Not Binding
**Diagnosis**:
1. `kubectl describe pvc <pvc-name>`
2. `kubectl get pv`
3. Check StorageClass

**Quick Fixes**:
- No matching PV: Create PV with matching requirements
- StorageClass issue: Fix StorageClass or create PV manually
- Access mode mismatch: Ensure PV access mode matches PVC

## YAML Templates for Quick Creation

### Quick Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: <name>
spec:
  containers:
  - name: <container-name>
    image: <image>
```

### Quick Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <name>
spec:
  replicas: 3
  selector:
    matchLabels:
      app: <app>
  template:
    metadata:
      labels:
        app: <app>
    spec:
      containers:
      - name: <container>
        image: <image>
```

### Quick Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: <name>
spec:
  selector:
    app: <app>
  ports:
  - port: 80
    targetPort: 80
```

### Quick ConfigMap
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: <name>
data:
  key: value
```

### Quick Secret
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: <name>
type: Opaque
stringData:
  username: admin
  password: secret
```

### Quick PVC
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <name>
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

## Imperative Commands (Faster for Exam)

### Create Resources
```bash
# Pod
kubectl run <name> --image=<image>

# Deployment
kubectl create deployment <name> --image=<image> --replicas=3

# Service
kubectl create service clusterip <name> --tcp=80:80

# ConfigMap
kubectl create configmap <name> --from-literal=key=value

# Secret
kubectl create secret generic <name> --from-literal=key=value

# Namespace
kubectl create namespace <name>
```

### Generate YAML
```bash
# Generate YAML from imperative command
kubectl create deployment <name> --image=<image> --dry-run=client -o yaml > deployment.yaml

# Edit and apply
kubectl apply -f deployment.yaml
```

### Quick Updates
```bash
# Update image
kubectl set image deployment/<name> <container>=<new-image>

# Scale
kubectl scale deployment <name> --replicas=5

# Rollout
kubectl rollout undo deployment/<name>
kubectl rollout status deployment/<name>
```

## Troubleshooting Workflow

### Step 1: Identify the Issue
```bash
# Get resource status
kubectl get <resource> <name>

# Describe for details
kubectl describe <resource> <name>

# Check events
kubectl get events --sort-by=.lastTimestamp
```

### Step 2: Diagnose Root Cause
```bash
# Check related resources
kubectl get pods,svc,deploy

# Check logs
kubectl logs <pod-name>

# Check configuration
kubectl get <resource> <name> -o yaml
```

### Step 3: Fix the Issue
```bash
# Edit resource
kubectl edit <resource> <name>

# Or patch
kubectl patch <resource> <name> -p '{"spec":{...}}'

# Or delete and recreate
kubectl delete <resource> <name>
kubectl apply -f <fixed-yaml>
```

### Step 4: Verify Fix
```bash
# Check status
kubectl get <resource> <name>

# Test functionality
kubectl exec <pod> -- <command>
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- <command>
```

## Time-Saving Tips

1. **Use Aliases**: `alias k=kubectl`
2. **Use Short Names**: `kubectl get po,svc,deploy`
3. **Use Output Formats**: `-o yaml`, `-o json`, `-o wide`
4. **Use Explain**: `kubectl explain pod.spec.containers`
5. **Use Dry-Run**: Generate YAML quickly
6. **Copy Existing**: `kubectl get <resource> <name> -o yaml > template.yaml`
7. **Use Labels**: `-l app=web` to filter resources
8. **Watch Mode**: `kubectl get pods -w` for real-time updates

## Exam Day Checklist

Before starting each task:
- [ ] Read requirements carefully
- [ ] Understand what needs to be created/fixed
- [ ] Check current state of resources
- [ ] Plan your approach
- [ ] Execute solution
- [ ] Verify it works
- [ ] Move to next task

Common mistakes to avoid:
- [ ] Not checking namespace
- [ ] Label mismatches
- [ ] Port mismatches
- [ ] Forgetting to verify
- [ ] Spending too long on one task
- [ ] Not using kubectl explain when stuck

Good luck! 🚀

