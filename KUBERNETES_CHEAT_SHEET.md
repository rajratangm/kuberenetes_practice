# Kubernetes Command Cheat Sheet - CKA Exam

Quick reference for all essential kubectl commands organized by category.

## Table of Contents
1. [Basic Commands](#basic-commands)
2. [Resource Management](#resource-management)
3. [Pods](#pods)
4. [Deployments](#deployments)
5. [Services](#services)
6. [ConfigMaps & Secrets](#configmaps--secrets)
7. [Storage](#storage)
8. [RBAC](#rbac)
9. [Networking](#networking)
10. [Troubleshooting](#troubleshooting)
11. [Node Management](#node-management)
12. [Debugging](#debugging)
13. [Output Formats](#output-formats)
14. [Time-Saving Tips](#time-saving-tips)

---

## Basic Commands

### Get Resources
```bash
# Get all resources
kubectl get all
kubectl get all -n <namespace>

# Get specific resource
kubectl get <resource> <name>
kubectl get pod <pod-name>
kubectl get deployment <deployment-name>

# Get with labels
kubectl get pods -l app=web
kubectl get pods --show-labels

# Get all namespaces
kubectl get pods --all-namespaces
kubectl get pods -A

# Get with wide output
kubectl get pods -o wide
kubectl get nodes -o wide
```

### Describe Resources
```bash
kubectl describe <resource> <name>
kubectl describe pod <pod-name>
kubectl describe node <node-name>
kubectl describe deployment <name>
```

### Delete Resources
```bash
kubectl delete <resource> <name>
kubectl delete pod <pod-name>
kubectl delete deployment <name>
kubectl delete -f <file.yaml>

# Delete by label
kubectl delete pods -l app=web

# Delete all in namespace
kubectl delete all --all -n <namespace>
```

### Apply/Create
```bash
# Apply YAML
kubectl apply -f <file.yaml>
kubectl apply -f <directory>/

# Create from YAML
kubectl create -f <file.yaml>

# Create imperatively
kubectl create deployment <name> --image=<image>
kubectl create service clusterip <name> --tcp=80:80
kubectl create namespace <name>
```

---

## Resource Management

### Scale Resources
```bash
# Scale deployment
kubectl scale deployment <name> --replicas=5

# Scale ReplicaSet
kubectl scale rs <name> --replicas=3

# Scale StatefulSet
kubectl scale statefulset <name> --replicas=5
```

### Edit Resources
```bash
kubectl edit <resource> <name>
kubectl edit pod <pod-name>
kubectl edit deployment <name>
```

### Patch Resources
```bash
# Patch deployment
kubectl patch deployment <name> -p '{"spec":{"replicas":5}}'

# Patch service
kubectl patch svc <name> -p '{"spec":{"sessionAffinity":"ClientIP"}}'

# Patch node
kubectl patch node <name> -p '{"spec":{"unschedulable":true}}'
```

### Set Resources
```bash
# Set image
kubectl set image deployment/<name> <container>=<new-image>

# Set resources
kubectl set resources deployment/<name> --limits=cpu=200m,memory=512Mi

# Set service account
kubectl set serviceaccount deployment/<name> <service-account>
```

---

## Pods

### Create Pods
```bash
# Imperative
kubectl run <pod-name> --image=<image>
kubectl run <pod-name> --image=<image> --restart=Never
kubectl run <pod-name> --image=<image> --port=80

# With command
kubectl run <pod-name> --image=<image> -- <command>

# Generate YAML
kubectl run <pod-name> --image=<image> --dry-run=client -o yaml > pod.yaml
```

### Pod Operations
```bash
# Get pod logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl logs <pod-name> -c <container-name>
kubectl logs <pod-name> --all-containers=true
kubectl logs -f <pod-name>  # Follow logs

# Execute command in pod
kubectl exec <pod-name> -- <command>
kubectl exec -it <pod-name> -- /bin/sh
kubectl exec <pod-name> -c <container> -- <command>

# Copy files
kubectl cp <pod-name>:/path/to/file /local/path
kubectl cp /local/path <pod-name>:/path/to/file

# Port forward
kubectl port-forward <pod-name> 8080:80
kubectl port-forward <pod-name> 8080:80 --address=0.0.0.0
```

### Pod Status
```bash
# Get pod status
kubectl get pod <pod-name>
kubectl get pod <pod-name> -o yaml
kubectl get pod <pod-name> -o json

# Check pod conditions
kubectl get pod <pod-name> -o jsonpath='{.status.conditions}'
```

---

## Deployments

### Deployment Operations
```bash
# Get deployment
kubectl get deployment <name>
kubectl get deploy <name>  # Short form

# Rollout management
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout history deployment/<name> --revision=2
kubectl rollout undo deployment/<name>
kubectl rollout undo deployment/<name> --to-revision=2
kubectl rollout pause deployment/<name>
kubectl rollout resume deployment/<name>
kubectl rollout restart deployment/<name>

# Check ReplicaSet
kubectl get rs -l app=<label>
kubectl describe rs <rs-name>
```

### Update Deployment
```bash
# Update image
kubectl set image deployment/<name> <container>=<new-image>

# Update multiple containers
kubectl set image deployment/<name> nginx=nginx:1.22 app=app:v2.0
```

---

## Services

### Service Operations
```bash
# Get services
kubectl get svc
kubectl get service <name>

# Get endpoints
kubectl get endpoints <service-name>
kubectl get ep <service-name>  # Short form

# Describe service
kubectl describe svc <service-name>

# Port forward to service
kubectl port-forward svc/<service-name> 8080:80
```

### Create Services
```bash
# Imperative
kubectl create service clusterip <name> --tcp=80:80
kubectl create service nodeport <name> --tcp=80:80 --node-port=30080
kubectl create service loadbalancer <name> --tcp=80:80

# Expose deployment as service
kubectl expose deployment <name> --port=80 --target-port=8080
kubectl expose deployment <name> --type=NodePort --port=80
```

---

## ConfigMaps & Secrets

### ConfigMaps
```bash
# Create ConfigMap
kubectl create configmap <name> --from-literal=key=value
kubectl create configmap <name> --from-literal=key1=value1 --from-literal=key2=value2
kubectl create configmap <name> --from-file=<file>
kubectl create configmap <name> --from-file=<directory>

# Get ConfigMap
kubectl get configmap <name>
kubectl get cm <name>  # Short form
kubectl get configmap <name> -o yaml

# Describe ConfigMap
kubectl describe configmap <name>
```

### Secrets
```bash
# Create Secret
kubectl create secret generic <name> --from-literal=key=value
kubectl create secret generic <name> --from-file=<file>
kubectl create secret docker-registry <name> \
  --docker-server=<server> \
  --docker-username=<user> \
  --docker-password=<pass>
kubectl create secret tls <name> --cert=<cert-file> --key=<key-file>

# Get Secret
kubectl get secret <name>
kubectl get secret <name> -o yaml

# Decode Secret
kubectl get secret <name> -o jsonpath='{.data.<key>}' | base64 -d
```

---

## Storage

### PersistentVolumes & PVCs
```bash
# Get PV and PVC
kubectl get pv
kubectl get pvc
kubectl get pv,pvc

# Describe
kubectl describe pv <pv-name>
kubectl describe pvc <pvc-name>

# Delete
kubectl delete pvc <pvc-name>
kubectl delete pv <pv-name>
```

### StorageClasses
```bash
# Get StorageClass
kubectl get storageclass
kubectl get sc  # Short form
kubectl describe storageclass <name>
```

---

## RBAC

### Roles & RoleBindings
```bash
# Get RBAC resources
kubectl get role,rolebinding
kubectl get clusterrole,clusterrolebinding

# Describe
kubectl describe role <name> -n <namespace>
kubectl describe rolebinding <name> -n <namespace>

# Create Role
kubectl create role <name> --verb=get,list --resource=pods -n <namespace>

# Create RoleBinding
kubectl create rolebinding <name> \
  --role=<role-name> \
  --serviceaccount=<namespace>:<sa-name> \
  -n <namespace>
```

### ServiceAccounts
```bash
# Get ServiceAccount
kubectl get serviceaccount
kubectl get sa  # Short form
kubectl get sa -A

# Create ServiceAccount
kubectl create serviceaccount <name> -n <namespace>
```

### Check Permissions
```bash
# Check if can perform action
kubectl auth can-i create pods
kubectl auth can-i create pods --as=system:serviceaccount:default:my-sa

# List all permissions
kubectl auth can-i --list
kubectl auth can-i --list --as=system:serviceaccount:<ns>:<sa> -n <namespace>
```

---

## Networking

### Ingress
```bash
# Get Ingress
kubectl get ingress
kubectl get ing  # Short form
kubectl describe ingress <name>
```

### NetworkPolicies
```bash
# Get NetworkPolicy
kubectl get networkpolicy
kubectl get netpol  # Short form
kubectl describe networkpolicy <name>
```

### DNS Testing
```bash
# Test DNS resolution
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- nslookup <service-name>
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- wget -O- http://<service-name>
```

---

## Troubleshooting

### Events
```bash
# Get events
kubectl get events
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get events --sort-by=.lastTimestamp
kubectl get events -n <namespace>

# Filter events
kubectl get events --field-selector type=Warning
kubectl get events --field-selector involvedObject.name=<pod-name>
kubectl get events --field-selector involvedObject.kind=Pod
```

### Resource Usage
```bash
# Top commands (requires metrics-server)
kubectl top nodes
kubectl top pods
kubectl top pods --all-namespaces
kubectl top pods --containers
kubectl top pods --sort-by=cpu
kubectl top pods --sort-by=memory
```

### Logs
```bash
# Pod logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl logs <pod-name> -c <container>
kubectl logs <pod-name> --all-containers=true
kubectl logs -f <pod-name>  # Follow
kubectl logs <pod-name> --timestamps
kubectl logs <pod-name> --since=1h
kubectl logs <pod-name> --since-time=2024-01-01T00:00:00Z

# Logs from label selector
kubectl logs -l app=web
kubectl logs -f -l app=web
```

---

## Node Management

### Node Operations
```bash
# Get nodes
kubectl get nodes
kubectl get nodes -o wide
kubectl describe node <node-name>

# Node management
kubectl cordon <node-name>
kubectl uncordon <node-name>
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data
kubectl drain <node-name> --force --ignore-daemonsets

# Node labels
kubectl label nodes <node-name> <key>=<value>
kubectl label nodes <node-name> <key>-
kubectl get nodes --show-labels

# Node taints
kubectl taint nodes <node-name> <key>=<value>:<effect>
kubectl taint nodes <node-name> <key>=<value>:<effect>-
kubectl describe node <node-name> | grep Taints
```

### Node Resources
```bash
# Check node resources
kubectl describe node <node-name> | grep -A 10 "Allocated resources"
kubectl get node <node-name> -o jsonpath='{.status.capacity}'
kubectl get node <node-name> -o jsonpath='{.status.allocatable}'
```

---

## Debugging

### Debug Pods
```bash
# Debug pod
kubectl debug <pod-name> -it --image=busybox:1.35

# Run temporary pod
kubectl run debug --image=busybox:1.35 --rm -it --restart=Never -- sh
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- <command>
```

### Get Resource Details
```bash
# Get YAML
kubectl get <resource> <name> -o yaml
kubectl get pod <pod-name> -o yaml > pod.yaml

# Get JSON
kubectl get <resource> <name> -o json

# Get specific fields
kubectl get pod <pod-name> -o jsonpath='{.spec.containers[0].image}'
kubectl get pod <pod-name> -o jsonpath='{.status.phase}'
kubectl get node <node-name> -o jsonpath='{.status.nodeInfo.kubeletVersion}'
```

### Explain Resources
```bash
# Explain resource
kubectl explain pod
kubectl explain pod.spec
kubectl explain pod.spec.containers
kubectl explain pod.spec.containers.resources
```

---

## Output Formats

### Custom Output
```bash
# YAML
kubectl get <resource> -o yaml

# JSON
kubectl get <resource> -o json

# Wide
kubectl get <resource> -o wide

# Custom columns
kubectl get pods -o custom-columns=NAME:.metadata.name,IMAGE:.spec.containers[0].image,NODE:.spec.nodeName

# JSONPath
kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.containers[0].image}{"\n"}{end}'

# Go template
kubectl get pods -o go-template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
```

### Save Output
```bash
# Save to file
kubectl get <resource> -o yaml > <file>.yaml
kubectl get all -n <namespace> -o yaml > backup.yaml
```

---

## Namespaces

### Namespace Operations
```bash
# Get namespaces
kubectl get namespaces
kubectl get ns  # Short form

# Create namespace
kubectl create namespace <name>

# Set default namespace
kubectl config set-context --current --namespace=<namespace>

# Get current namespace
kubectl config view --minify | grep namespace
```

---

## Jobs & CronJobs

### Jobs
```bash
# Get Jobs
kubectl get jobs
kubectl get job <name>

# Get Job logs
kubectl logs job/<job-name>
kubectl logs -l job-name=<job-name>

# Delete Job
kubectl delete job <name>
```

### CronJobs
```bash
# Get CronJobs
kubectl get cronjobs
kubectl get cj  # Short form

# Manually trigger CronJob
kubectl create job --from=cronjob/<cronjob-name> <job-name>

# Suspend/Resume
kubectl patch cronjob <name> -p '{"spec":{"suspend":true}}'
kubectl patch cronjob <name> -p '{"spec":{"suspend":false}}'
```

---

## StatefulSets & DaemonSets

### StatefulSets
```bash
# Get StatefulSet
kubectl get statefulsets
kubectl get sts  # Short form

# Scale StatefulSet
kubectl scale statefulset <name> --replicas=5
```

### DaemonSets
```bash
# Get DaemonSet
kubectl get daemonsets
kubectl get ds  # Short form

# Update DaemonSet
kubectl set image daemonset/<name> <container>=<new-image>
```

---

## Resource Quotas & Limits

### ResourceQuota
```bash
# Get ResourceQuota
kubectl get resourcequota
kubectl get quota  # Short form
kubectl describe resourcequota <name> -n <namespace>
```

### LimitRange
```bash
# Get LimitRange
kubectl get limitrange
kubectl get limits  # Short form
kubectl describe limitrange <name> -n <namespace>
```

---

## Pod Disruption Budgets

```bash
# Get PDB
kubectl get poddisruptionbudget
kubectl get pdb  # Short form
kubectl describe pdb <name>
```

---

## Cluster Information

### Version & Info
```bash
# Cluster info
kubectl cluster-info
kubectl version
kubectl version --short

# API resources
kubectl api-resources
kubectl api-resources --verbs=list,get
kubectl api-resources --api-group=apps
```

### Context Management
```bash
# Get contexts
kubectl config get-contexts
kubectl config current-context

# Switch context
kubectl config use-context <context-name>

# View config
kubectl config view
kubectl config view --minify
```

---

## Time-Saving Tips

### Aliases
```bash
# Add to ~/.bashrc or ~/.zshrc
alias k=kubectl
alias kg='kubectl get'
alias kd='kubectl describe'
alias ka='kubectl apply'
alias kdel='kubectl delete'
alias kl='kubectl logs'
alias ke='kubectl exec -it'
```

### Short Names
```bash
# Resource short names
kubectl get po  # pods
kubectl get svc  # services
kubectl get deploy  # deployments
kubectl get rs  # replicasets
kubectl get cm  # configmaps
kubectl get secret  # secrets
kubectl get ns  # namespaces
kubectl get pv  # persistentvolumes
kubectl get pvc  # persistentvolumeclaims
kubectl get sc  # storageclasses
kubectl get ing  # ingress
kubectl get netpol  # networkpolicies
kubectl get sa  # serviceaccounts
kubectl get sts  # statefulsets
kubectl get ds  # daemonsets
kubectl get cj  # cronjobs
kubectl get pdb  # poddisruptionbudgets
```

### Dry Run
```bash
# Generate YAML without creating
kubectl run <name> --image=<image> --dry-run=client -o yaml
kubectl create deployment <name> --image=<image> --dry-run=client -o yaml
kubectl create service clusterip <name> --tcp=80:80 --dry-run=client -o yaml
```

### Watch Mode
```bash
# Watch resources
kubectl get pods -w
kubectl get pods -w -l app=web
kubectl get events -w
```

### Multiple Resources
```bash
# Get multiple resource types
kubectl get pods,svc,deploy
kubectl get all
kubectl get all,configmap,secret -n <namespace>
```

---

## Quick Reference Card

### Most Used Commands
```bash
# Get resources
kubectl get <resource> <name>
kubectl get pods -o wide
kubectl get all -n <namespace>

# Describe
kubectl describe <resource> <name>

# Logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous

# Execute
kubectl exec -it <pod-name> -- /bin/sh

# Apply
kubectl apply -f <file.yaml>

# Delete
kubectl delete <resource> <name>

# Edit
kubectl edit <resource> <name>

# Events
kubectl get events --sort-by=.lastTimestamp
```

### Troubleshooting Sequence
```bash
# 1. Get status
kubectl get <resource> <name>

# 2. Describe for details
kubectl describe <resource> <name>

# 3. Check logs
kubectl logs <pod-name>

# 4. Check events
kubectl get events --field-selector involvedObject.name=<name>

# 5. Check related resources
kubectl get <related-resources>
```

---

## Exam Day Quick Reference

### 1-Minute Tasks
```bash
kubectl get pods
kubectl logs <pod-name>
kubectl describe pod <pod-name>
kubectl get events
kubectl scale deployment <name> --replicas=5
```

### 2-Minute Tasks
```bash
kubectl create configmap <name> --from-literal=key=value
kubectl create secret generic <name> --from-literal=key=value
kubectl set image deployment/<name> <container>=<image>
kubectl rollout undo deployment/<name>
```

### 3-Minute Tasks
```bash
kubectl create deployment <name> --image=<image> --replicas=3
kubectl expose deployment <name> --port=80
kubectl create role <name> --verb=create --resource=pods
kubectl create rolebinding <name> --role=<role> --serviceaccount=<ns>:<sa>
```

---

## Practice Tips

1. **Use short names**: `po`, `svc`, `deploy` save time
2. **Use `-o yaml`**: Get existing resource as template
3. **Use `--dry-run=client -o yaml`**: Generate YAML quickly
4. **Use `kubectl explain`**: When unsure about YAML structure
5. **Use labels**: `-l app=web` to filter resources
6. **Use `-A` or `--all-namespaces`**: When needed
7. **Use `-w`**: Watch mode for real-time updates
8. **Use `kubectl auth can-i`**: Test permissions quickly

---

**Remember**: During the CKA exam, you can access Kubernetes documentation. Use `kubectl explain` liberally!

Good luck! 🚀

