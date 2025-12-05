# Scenario 1: Core Concepts

## Scenario 1.1: Create and Manage Pods

### Question 1.1.1
Create a Pod named `web-server` in the `default` namespace that:
- Uses the image `nginx:1.21`
- Has a label `app=web` and `env=production`
- Exposes port 80
- Has resource limits: CPU 500m, Memory 512Mi
- Has resource requests: CPU 200m, Memory 256Mi

**Solution Steps:**
1. Check the solution YAML in `solutions/pod-web-server.yaml`
2. Apply using: `kubectl apply -f solutions/pod-web-server.yaml`
3. Verify: `kubectl get pod web-server -o wide`
4. Check logs: `kubectl logs web-server`

### Question 1.1.2
Create a multi-container Pod named `app-logger` with:
- Container 1: `app` using image `busybox:1.35` that runs `sleep 3600`
- Container 2: `logger` using image `busybox:1.35` that runs `tail -f /var/log/app.log`
- Both containers share a volume at `/var/log`
- Use an emptyDir volume

**Solution Steps:**
1. Check `solutions/pod-multi-container.yaml`
2. Apply and verify the containers are running

### Question 1.1.3
Create a Pod with a ConfigMap mounted as a volume:
- Pod name: `config-app`
- Image: `busybox:1.35`
- Command: `env`
- ConfigMap name: `app-config` with data:
  - `database_url=postgresql://localhost:5432/mydb`
  - `api_key=secret123`
- Mount the ConfigMap at `/etc/config`

**Solution Steps:**
1. First create the ConfigMap (see `solutions/configmap-app-config.yaml`)
2. Then create the Pod (see `solutions/pod-config-app.yaml`)
3. Verify: `kubectl exec config-app -- env | grep -E "database_url|api_key"`

## Scenario 1.2: Deployments and ReplicaSets

### Question 1.2.1
Create a Deployment named `frontend` with:
- 3 replicas
- Image: `nginx:1.21`
- Labels: `app=frontend`, `tier=web`
- Rolling update strategy with maxSurge=1 and maxUnavailable=0
- Resource requests: CPU 100m, Memory 128Mi

**Solution Steps:**
1. Check `solutions/deployment-frontend.yaml`
2. Apply: `kubectl apply -f solutions/deployment-frontend.yaml`
3. Verify: `kubectl get deployment frontend`
4. Check ReplicaSet: `kubectl get rs -l app=frontend`
5. Scale: `kubectl scale deployment frontend --replicas=5`

### Question 1.2.2
Update the `frontend` deployment to:
- Use image `nginx:1.22`
- Add an environment variable `ENV=production`
- Add a readiness probe on port 80
- Add a liveness probe on port 80

**Solution Steps:**
1. Check `solutions/deployment-frontend-updated.yaml`
2. Apply the update
3. Watch the rolling update: `kubectl rollout status deployment frontend`
4. Check rollout history: `kubectl rollout history deployment frontend`

### Question 1.2.3
Create a Deployment with:
- Name: `api-server`
- Image: `httpd:2.4`
- Replicas: 2
- Rolling update with maxSurge=2, maxUnavailable=1
- Add a label selector: `app=api`
- Pod template labels: `app=api`, `version=v1`

**Solution Steps:**
1. Check `solutions/deployment-api-server.yaml`
2. Apply and verify
3. Test rolling update by changing image version

## Scenario 1.3: ReplicaSets

### Question 1.3.1
Create a ReplicaSet named `web-rs` that:
- Maintains 4 replicas
- Uses image `nginx:1.21`
- Has selector: `app=web`, `component=frontend`
- Pods should have labels matching the selector

**Solution Steps:**
1. Check `solutions/replicaset-web.yaml`
2. Apply and verify
3. Delete a pod and watch it recreate: `kubectl delete pod <pod-name>`

### Question 1.3.2
Manually scale the ReplicaSet to 6 replicas and verify.

**Solution Steps:**
1. Edit: `kubectl edit replicaset web-rs` (change replicas to 6)
2. Or use: `kubectl scale replicaset web-rs --replicas=6`
3. Verify: `kubectl get pods -l app=web,component=frontend`

### Question 1.1.4: Pod with Health Probes
Create a Pod named `healthy-app` with:
- Image: `nginx:1.21`
- Readiness probe: HTTP GET on port 80, path `/`, initial delay 5s, period 10s
- Liveness probe: HTTP GET on port 80, path `/`, initial delay 15s, period 20s
- Startup probe: HTTP GET on port 80, path `/`, initial delay 0s, period 5s, failure threshold 30

**Solution Steps:**
1. Check `solutions/pod-health-probes.yaml`
2. Apply and verify probes are working
3. Check: `kubectl describe pod healthy-app | grep -A 10 "Liveness\|Readiness\|Startup"`

### Question 1.1.5: Pod with Security Context
Create a Pod named `secure-pod` with:
- Image: `nginx:1.21`
- Run as non-root user (UID 1000)
- Run as group 2000
- Read-only root filesystem
- Drop all capabilities except NET_BIND_SERVICE

**Solution Steps:**
1. Check `solutions/pod-security-context.yaml`
2. Apply and verify security context
3. Test: `kubectl exec secure-pod -- id`

### Question 1.1.6: Pod with Resource Quotas
Create a Pod that respects namespace resource quotas:
- Pod name: `quota-pod`
- Image: `nginx:1.21`
- CPU request: 100m, limit: 200m
- Memory request: 128Mi, limit: 256Mi
- Ensure it fits within namespace quota

**Solution Steps:**
1. Check namespace quota first: `kubectl get resourcequota -n <namespace>`
2. Create pod with appropriate resources
3. Verify: `kubectl describe pod quota-pod | grep -A 5 "Limits\|Requests"`

### Question 1.1.7: Pod with Node Selector
Create a Pod that only runs on nodes with label `node-type=compute`:
- Pod name: `compute-pod`
- Image: `nginx:1.21`
- Node selector: `node-type=compute`

**Solution Steps:**
1. Label a node first: `kubectl label nodes <node-name> node-type=compute`
2. Check `solutions/pod-node-selector.yaml`
3. Apply and verify pod is scheduled on correct node

### Question 1.1.8: Pod with Tolerations
Create a Pod that can tolerate taint `dedicated=special:NoSchedule`:
- Pod name: `tolerated-pod`
- Image: `nginx:1.21`
- Add appropriate toleration

**Solution Steps:**
1. Taint a node: `kubectl taint nodes <node-name> dedicated=special:NoSchedule`
2. Check `solutions/pod-with-toleration.yaml`
3. Apply and verify pod schedules on tainted node

### Question 1.1.9: Pod with Init Containers
Create a Pod with:
- Name: `init-pod`
- Init container 1: Wait for service `web-service` to be ready
- Init container 2: Download config file
- Main container: `nginx:1.21` that uses the config

**Solution Steps:**
1. Check `solutions/pod-init-containers.yaml`
2. Apply and watch init containers complete
3. Verify: `kubectl describe pod init-pod | grep -A 10 "Init Containers"`

### Question 1.1.10: Pod with Lifecycle Hooks
Create a Pod with:
- Name: `lifecycle-pod`
- Image: `nginx:1.21`
- PreStop hook: Sleep for 10 seconds
- PostStart hook: Write a file to `/tmp/started`

**Solution Steps:**
1. Check `solutions/pod-lifecycle-hooks.yaml`
2. Apply and verify hooks execute
3. Check logs and file creation

## Scenario 1.2: Deployments and ReplicaSets

### Question 1.2.4: Deployment with Rolling Update
Create a Deployment and perform rolling update:
- Name: `rolling-app`
- Initial image: `nginx:1.21`
- Replicas: 5
- Strategy: RollingUpdate with maxSurge=2, maxUnavailable=1
- Update to `nginx:1.22` and watch the rollout

**Solution Steps:**
1. Create deployment: `kubectl create deployment rolling-app --image=nginx:1.21 --replicas=5`
2. Update: `kubectl set image deployment/rolling-app nginx=nginx:1.22`
3. Watch: `kubectl rollout status deployment/rolling-app`
4. Verify: `kubectl get pods -l app=rolling-app`

### Question 1.2.5: Deployment Rollback
Create a Deployment, update it, then rollback:
- Name: `rollback-app`
- Initial image: `nginx:1.21`
- Update to `nginx:1.22`
- Rollback to previous version
- Rollback to specific revision

**Solution Steps:**
1. Create and update deployment
2. Check history: `kubectl rollout history deployment/rollback-app`
3. Rollback: `kubectl rollout undo deployment/rollback-app`
4. Rollback to revision: `kubectl rollout undo deployment/rollback-app --to-revision=1`

### Question 1.2.6: Deployment with Pod Disruption Budget
Create a Deployment with PDB:
- Deployment: `pdb-app` with 5 replicas
- PDB: Min available 3 pods
- Test by draining a node

**Solution Steps:**
1. Create deployment with 5 replicas
2. Create PDB (see `08-configuration/solutions/pdb-web.yaml`)
3. Drain a node and verify at least 3 pods remain

### Question 1.2.7: Deployment with Pod Anti-Affinity
Create a Deployment that ensures pods are on different nodes:
- Name: `distributed-app`
- Replicas: 3
- Pod anti-affinity: Required, topology key `kubernetes.io/hostname`

**Solution Steps:**
1. Check `08-configuration/solutions/deployment-pod-antiaffinity.yaml`
2. Apply and verify pods are on different nodes: `kubectl get pods -o wide`

### Question 1.2.8: Deployment with Pod Affinity
Create a Deployment with pod affinity:
- Name: `affinity-app`
- Preferred affinity to pods with label `app=cache`
- Ensure pods prefer to be on same node as cache pods

**Solution Steps:**
1. Create cache deployment first
2. Check `08-configuration/solutions/deployment-pod-affinity.yaml`
3. Apply and verify pod placement

### Question 1.2.9: Deployment with Environment Variables
Create a Deployment with:
- Name: `env-app`
- Environment variables from ConfigMap and Secret
- Some variables set directly
- Use envFrom for multiple keys

**Solution Steps:**
1. Create ConfigMap and Secret
2. Create deployment with env vars
3. Verify: `kubectl exec <pod> -- env`

### Question 1.2.10: Deployment Scaling
Create a Deployment and practice scaling:
- Name: `scale-app`
- Initial replicas: 2
- Scale up to 5
- Scale down to 1
- Use HPA (if metrics-server available)

**Solution Steps:**
1. Create deployment
2. Scale: `kubectl scale deployment scale-app --replicas=5`
3. Verify: `kubectl get deployment scale-app`
4. Autoscale: `kubectl autoscale deployment scale-app --min=2 --max=10 --cpu-percent=80`

## Scenario 1.3: ReplicaSets

### Question 1.3.3: ReplicaSet with Selector
Create a ReplicaSet and verify selector matching:
- Name: `selector-rs`
- Replicas: 3
- Selector: `app=web`, `env=prod`
- Ensure pod labels match exactly

**Solution Steps:**
1. Check `solutions/replicaset-web.yaml`
2. Apply and verify selector matches
3. Test: Change pod label and see ReplicaSet create new pod

### Question 1.3.4: ReplicaSet Manual Pod Management
Create a ReplicaSet, manually create a pod with matching labels, and observe behavior.

**Solution Steps:**
1. Create ReplicaSet with 2 replicas
2. Manually create pod with same labels
3. Observe ReplicaSet adjusts to desired count

### Question 1.3.5: ReplicaSet vs Deployment
Understand the difference:
- Create a ReplicaSet directly
- Create a Deployment (which creates ReplicaSet)
- Compare and understand when to use each

**Solution Steps:**
1. Create both and compare
2. Understand Deployment manages ReplicaSet
3. Check: `kubectl get rs` shows ReplicaSet created by Deployment

## Scenario 1.4: Pod Lifecycle and States

### Question 1.4.1: Understand Pod States
Create scenarios to see different pod states:
- Pending (resource constraints)
- Running (normal)
- Succeeded (job completion)
- Failed (container error)
- CrashLoopBackOff (restart loop)

**Solution Steps:**
1. Create pods in different states
2. Observe: `kubectl get pods`
3. Understand each state and when it occurs

### Question 1.4.2: Pod Termination
Understand graceful termination:
- Create a pod with preStop hook
- Delete the pod
- Observe termination grace period
- Verify preStop hook executes

**Solution Steps:**
1. Create pod with preStop hook
2. Delete: `kubectl delete pod <name>`
3. Watch: `kubectl get pod <name> -w`
4. Verify hook execution

## Practice Commands

```bash
# Get all pods with labels
kubectl get pods --show-labels

# Describe a pod
kubectl describe pod <pod-name>

# Get pod logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl logs <pod-name> -c <container-name>

# Execute command in pod
kubectl exec -it <pod-name> -- /bin/sh
kubectl exec <pod-name> -- <command>

# Watch resources
kubectl get pods -w
kubectl get deployment -w

# Get YAML output
kubectl get pod <pod-name> -o yaml
kubectl get deployment <name> -o yaml

# Delete resources
kubectl delete pod <pod-name>
kubectl delete deployment <deployment-name>
kubectl delete rs <replicaset-name>

# Scale resources
kubectl scale deployment <name> --replicas=5
kubectl scale rs <name> --replicas=3

# Rollout management
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>
kubectl rollout pause deployment/<name>
kubectl rollout resume deployment/<name>

# Get events
kubectl get events --sort-by=.metadata.creationTimestamp

# Check resource usage
kubectl top pod <pod-name>
kubectl top pods
```

