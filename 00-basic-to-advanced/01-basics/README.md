# Level 1: Basics - Foundation Concepts

Master the fundamental Kubernetes concepts. These are the building blocks for everything else.

## Topic 1: Understanding Kubernetes Basics

### Question 1.1: What is a Pod?
**Task**: Create your first pod and understand what it is.

**Steps**:
1. Create a pod named `my-first-pod` using image `nginx:1.21`
2. Check the pod status
3. Describe the pod to see its details
4. Get the pod logs

**Solution**:
```bash
# Create pod
kubectl run my-first-pod --image=nginx:1.21

# Check status
kubectl get pods

# Describe pod
kubectl describe pod my-first-pod

# Get logs
kubectl logs my-first-pod
```

**Expected Time**: 2 minutes

---

### Question 1.2: Understanding Namespaces
**Task**: Work with namespaces.

**Steps**:
1. List all namespaces
2. Create a namespace called `practice`
3. Create a pod in the `practice` namespace
4. List pods in the `practice` namespace
5. Set `practice` as default namespace

**Solution**:
```bash
# List namespaces
kubectl get namespaces
kubectl get ns

# Create namespace
kubectl create namespace practice

# Create pod in namespace
kubectl run test-pod --image=busybox:1.35 --namespace=practice

# List pods in namespace
kubectl get pods -n practice

# Set default namespace
kubectl config set-context --current --namespace=practice
```

**Expected Time**: 3 minutes

---

### Question 1.3: Understanding Labels
**Task**: Work with labels on pods.

**Steps**:
1. Create a pod with labels `app=web` and `env=dev`
2. List pods showing labels
3. Find pods with label `app=web`
4. Add a new label `version=v1` to the pod
5. Remove the `env` label

**Solution**:
```bash
# Create pod with labels
kubectl run web-pod --image=nginx:1.21 --labels="app=web,env=dev"

# List with labels
kubectl get pods --show-labels

# Find by label
kubectl get pods -l app=web

# Add label
kubectl label pod web-pod version=v1

# Remove label
kubectl label pod web-pod env-
```

**Expected Time**: 3 minutes

---

### Question 1.4: Understanding Pod Lifecycle
**Task**: Observe pod states.

**Steps**:
1. Create a pod and watch it transition through states
2. Create a pod that will fail (wrong image)
3. Observe the pod state
4. Check pod events
5. Delete the failed pod

**Solution**:
```bash
# Create and watch
kubectl run test-pod --image=nginx:1.21
kubectl get pods -w

# Create failing pod
kubectl run fail-pod --image=invalid-image:latest

# Check status
kubectl get pods
kubectl describe pod fail-pod

# Check events
kubectl get events --sort-by=.lastTimestamp

# Delete
kubectl delete pod fail-pod
```

**Expected Time**: 4 minutes

---

## Topic 2: Basic Pod Operations

### Question 2.1: Create Pod with Command
**Task**: Create a pod that runs a specific command.

**Steps**:
1. Create a pod that runs `sleep 3600`
2. Execute a command in the running pod
3. Check the pod is running the command

**Solution**:
```bash
# Create pod with command
kubectl run sleep-pod --image=busybox:1.35 -- sleep 3600

# Execute command
kubectl exec sleep-pod -- ps aux

# Verify
kubectl get pod sleep-pod
```

**Expected Time**: 2 minutes

---

### Question 2.2: Pod Logs
**Task**: Work with pod logs.

**Steps**:
1. Create a pod that outputs logs
2. View the logs
3. Follow logs in real-time
4. View logs with timestamps

**Solution**:
```bash
# Create pod
kubectl run log-pod --image=busybox:1.35 -- sh -c 'while true; do echo $(date); sleep 5; done'

# View logs
kubectl logs log-pod

# Follow logs
kubectl logs -f log-pod

# With timestamps
kubectl logs log-pod --timestamps
```

**Expected Time**: 3 minutes

---

### Question 2.3: Execute Commands in Pod
**Task**: Execute commands inside a pod.

**Steps**:
1. Create a pod
2. Execute a simple command (`echo "Hello"`)
3. Get an interactive shell
4. List files in the pod
5. Check environment variables

**Solution**:
```bash
# Create pod
kubectl run exec-pod --image=busybox:1.35 -- sleep 3600

# Execute command
kubectl exec exec-pod -- echo "Hello"

# Interactive shell
kubectl exec -it exec-pod -- /bin/sh

# List files
kubectl exec exec-pod -- ls -la

# Environment variables
kubectl exec exec-pod -- env
```

**Expected Time**: 3 minutes

---

### Question 2.4: Port Forwarding
**Task**: Access a pod's port from your local machine.

**Steps**:
1. Create a pod running nginx
2. Port forward to access nginx
3. Test the connection
4. Port forward in background

**Solution**:
```bash
# Create pod
kubectl run nginx-pod --image=nginx:1.21

# Port forward
kubectl port-forward nginx-pod 8080:80

# In another terminal, test
curl http://localhost:8080

# Background (add & at end)
kubectl port-forward nginx-pod 8080:80 &
```

**Expected Time**: 3 minutes

---

## Topic 3: Basic Deployments

### Question 3.1: Create Your First Deployment
**Task**: Create a deployment and understand ReplicaSets.

**Steps**:
1. Create a deployment with 3 replicas
2. Check the deployment status
3. Check the ReplicaSet created
4. Check the pods created
5. Understand the relationship

**Solution**:
```bash
# Create deployment
kubectl create deployment web --image=nginx:1.21 --replicas=3

# Check deployment
kubectl get deployment web
kubectl describe deployment web

# Check ReplicaSet
kubectl get rs
kubectl get rs -l app=web

# Check pods
kubectl get pods -l app=web

# Understand relationship
kubectl get deploy,rs,pods -l app=web
```

**Expected Time**: 4 minutes

---

### Question 3.2: Scale a Deployment
**Task**: Scale deployments up and down.

**Steps**:
1. Create a deployment with 2 replicas
2. Scale it up to 5 replicas
3. Scale it down to 1 replica
4. Verify scaling worked
5. Check pod distribution across nodes

**Solution**:
```bash
# Create deployment
kubectl create deployment app --image=nginx:1.21 --replicas=2

# Scale up
kubectl scale deployment app --replicas=5

# Scale down
kubectl scale deployment app --replicas=1

# Verify
kubectl get deployment app
kubectl get pods -l app=app

# Check distribution
kubectl get pods -l app=app -o wide
```

**Expected Time**: 3 minutes

---

### Question 3.3: Update Deployment Image
**Task**: Update a deployment to use a new image.

**Steps**:
1. Create a deployment
2. Update the image to a new version
3. Watch the rolling update
4. Check rollout status
5. Verify all pods are updated

**Solution**:
```bash
# Create deployment
kubectl create deployment web --image=nginx:1.21 --replicas=3

# Update image
kubectl set image deployment/web nginx=nginx:1.22

# Watch update
kubectl get pods -l app=web -w

# Check status
kubectl rollout status deployment/web

# Verify
kubectl get pods -l app=web
kubectl describe pod <pod-name> | grep Image
```

**Expected Time**: 4 minutes

---

### Question 3.4: Rollback a Deployment
**Task**: Rollback a deployment to previous version.

**Steps**:
1. Create a deployment
2. Update it twice
3. Check rollout history
4. Rollback to previous version
5. Rollback to specific revision

**Solution**:
```bash
# Create deployment
kubectl create deployment app --image=nginx:1.21

# Update twice
kubectl set image deployment/app nginx=nginx:1.22
kubectl set image deployment/app nginx=nginx:1.23

# Check history
kubectl rollout history deployment/app

# Rollback to previous
kubectl rollout undo deployment/app

# Rollback to specific revision
kubectl rollout undo deployment/app --to-revision=1

# Verify
kubectl rollout status deployment/app
```

**Expected Time**: 4 minutes

---

## Topic 4: Basic Services

### Question 4.1: Create Your First Service
**Task**: Create a service to expose pods.

**Steps**:
1. Create a deployment
2. Create a ClusterIP service
3. Check service endpoints
4. Test connectivity from a pod
5. Understand service selector

**Solution**:
```bash
# Create deployment
kubectl create deployment web --image=nginx:1.21 --replicas=3

# Create service
kubectl expose deployment web --port=80 --target-port=80

# Check service
kubectl get svc web
kubectl describe svc web

# Check endpoints
kubectl get endpoints web

# Test connectivity
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- wget -O- http://web:80
```

**Expected Time**: 4 minutes

---

### Question 4.2: Service Types
**Task**: Create different service types.

**Steps**:
1. Create a ClusterIP service
2. Create a NodePort service
3. Create a LoadBalancer service
4. Understand the differences
5. Check service details

**Solution**:
```bash
# Create deployment first
kubectl create deployment app --image=nginx:1.21

# ClusterIP (default)
kubectl expose deployment app --name=app-clusterip --port=80

# NodePort
kubectl expose deployment app --name=app-nodeport --type=NodePort --port=80

# LoadBalancer
kubectl expose deployment app --name=app-lb --type=LoadBalancer --port=80

# Check all
kubectl get svc
kubectl describe svc app-nodeport
```

**Expected Time**: 5 minutes

---

### Question 4.3: Service Selector
**Task**: Understand how service selector works.

**Steps**:
1. Create pods with specific labels
2. Create a service with matching selector
3. Verify endpoints are created
4. Change pod labels and observe
5. Fix the selector mismatch

**Solution**:
```bash
# Create pods with labels
kubectl run pod1 --image=nginx:1.21 --labels="app=web,tier=frontend"
kubectl run pod2 --image=nginx:1.21 --labels="app=web,tier=frontend"

# Create service
kubectl create service clusterip web-svc --tcp=80:80
kubectl patch svc web-svc -p '{"spec":{"selector":{"app":"web","tier":"frontend"}}}'

# Check endpoints
kubectl get endpoints web-svc

# Change label
kubectl label pod pod1 app=api --overwrite

# Check endpoints again
kubectl get endpoints web-svc

# Fix
kubectl label pod pod1 app=web --overwrite
```

**Expected Time**: 5 minutes

---

## Topic 5: Basic ConfigMaps

### Question 5.1: Create ConfigMap
**Task**: Create and use ConfigMaps.

**Steps**:
1. Create a ConfigMap from literal values
2. Create a ConfigMap from a file
3. List ConfigMaps
4. View ConfigMap contents
5. Use ConfigMap in a pod as environment variable

**Solution**:
```bash
# From literal
kubectl create configmap app-config --from-literal=env=production --from-literal=log_level=info

# From file
echo "server.port=8080" > config.properties
kubectl create configmap file-config --from-file=config.properties

# List
kubectl get configmap
kubectl get cm

# View
kubectl get configmap app-config -o yaml
kubectl describe configmap app-config

# Use in pod
kubectl run test --image=busybox:1.35 --env="ENV=$(kubectl get configmap app-config -o jsonpath='{.data.env}')" -- sleep 3600
```

**Expected Time**: 5 minutes

---

### Question 5.2: ConfigMap in Pod
**Task**: Mount ConfigMap in a pod.

**Steps**:
1. Create a ConfigMap
2. Create a pod that mounts ConfigMap as volume
3. Create a pod that uses ConfigMap as env vars
4. Verify both methods work
5. Check the mounted files

**Solution**:
See `solutions/configmap-pod-volume.yaml` and `solutions/configmap-pod-env.yaml`

**Expected Time**: 6 minutes

---

## Topic 6: Basic Secrets

### Question 6.1: Create Secret
**Task**: Create and use secrets.

**Steps**:
1. Create a secret from literal values
2. View the secret (note it's base64 encoded)
3. Decode the secret value
4. Use secret in pod as environment variable
5. Verify pod can access the secret

**Solution**:
```bash
# Create secret
kubectl create secret generic app-secret --from-literal=username=admin --from-literal=password=secret123

# View (encoded)
kubectl get secret app-secret -o yaml

# Decode
kubectl get secret app-secret -o jsonpath='{.data.password}' | base64 -d

# Use in pod (see solutions/secret-pod-env.yaml)
```

**Expected Time**: 5 minutes

---

## Practice Exercises

### Exercise 1: Complete Basic Setup
Create a complete setup:
1. Namespace: `basic-practice`
2. Deployment: `web-app` with 2 replicas, image `nginx:1.21`
3. Service: Expose the deployment on port 80
4. ConfigMap: `app-config` with `env=dev`
5. Secret: `app-secret` with `api_key=abc123`
6. Pod: Use both ConfigMap and Secret

**Expected Time**: 10 minutes

### Exercise 2: Troubleshooting Basics
Fix these issues:
1. Pod in Pending state
2. Service with no endpoints
3. Deployment not scaling
4. ConfigMap not loading

**Expected Time**: 15 minutes

---

## Solutions

All solutions are in the `solutions/` folder with detailed YAML files and step-by-step instructions.

## Next Level

Once you've mastered all basics questions, move to:
- **02-intermediate/** - Practical skills and common scenarios

