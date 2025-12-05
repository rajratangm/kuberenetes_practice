# Scenario 6: Cluster Maintenance

## Scenario 6.1: Node Management

### Question 6.1.1: Cordon and Drain Node
Cordon a node to prevent new pods from being scheduled, then drain it.

**Solution Steps:**
1. Cordon node: `kubectl cordon <node-name>`
2. Verify: `kubectl get nodes` (should show SchedulingDisabled)
3. Drain node: `kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data`
4. Uncordon after maintenance: `kubectl uncordon <node-name>`

### Question 6.1.2: Add Labels and Taints
Add labels and taints to a node:
- Label: `node-type=compute`
- Taint: `dedicated=special:NoSchedule`

**Solution Steps:**
1. Add label: `kubectl label nodes <node-name> node-type=compute`
2. Add taint: `kubectl taint nodes <node-name> dedicated=special:NoSchedule`
3. Verify: `kubectl describe node <node-name>`
4. Remove taint: `kubectl taint nodes <node-name> dedicated=special:NoSchedule-`

### Question 6.1.3: Remove Node Labels
Remove a label from a node:
- Remove label `node-type` from node

**Solution Steps:**
1. Remove label: `kubectl label nodes <node-name> node-type-`
2. Verify: `kubectl get nodes <node-name> --show-labels`

### Question 6.1.4: Update Node Taints
Update node taint effect:
- Change taint from `NoSchedule` to `PreferNoSchedule`
- Or change taint value

**Solution Steps:**
1. Remove old taint: `kubectl taint nodes <node-name> dedicated=special:NoSchedule-`
2. Add new taint: `kubectl taint nodes <node-name> dedicated=special:PreferNoSchedule`
3. Verify: `kubectl describe node <node-name>`

### Question 6.1.5: Node Conditions
Check and understand node conditions:
- Ready, MemoryPressure, DiskPressure, PIDPressure, NetworkUnavailable

**Solution Steps:**
1. Check: `kubectl describe node <node-name> | grep -A 10 Conditions`
2. Understand each condition type
3. Identify issues from conditions

### Question 6.1.6: Node Resources
Check node resource capacity and allocatable:
- CPU capacity and allocatable
- Memory capacity and allocatable
- Pod capacity

**Solution Steps:**
1. Check: `kubectl describe node <node-name> | grep -A 10 "Capacity\|Allocatable"`
2. Understand difference between capacity and allocatable
3. Check: `kubectl get node <node-name> -o jsonpath='{.status.capacity}'`

## Scenario 6.2: Cluster Upgrades

### Question 6.2.1: Check Current Version
Check Kubernetes version of cluster and nodes.

**Solution Steps:**
1. Cluster version: `kubectl version`
2. Node versions: `kubectl get nodes -o wide`
3. Check kubelet version on node: `kubectl get node <node-name> -o jsonpath='{.status.nodeInfo.kubeletVersion}'`

### Question 6.2.2: Upgrade Process (Simulation)
Understand the upgrade process:
1. Upgrade control plane
2. Upgrade kubelet on nodes
3. Verify cluster health

**Note:** Actual upgrades depend on installation method (kubeadm, managed service, etc.)

### Question 6.2.3: Check Component Versions
Check versions of all Kubernetes components:
- API server version
- kubelet version
- kube-proxy version
- Container runtime version

**Solution Steps:**
1. Check node info: `kubectl get node <node-name> -o yaml | grep -A 5 nodeInfo`
2. Check API server: `kubectl version --short`
3. Check kubelet: `kubectl get node <node-name> -o jsonpath='{.status.nodeInfo.kubeletVersion}'`

### Question 6.2.4: Version Skew Policy
Understand Kubernetes version skew policy:
- Control plane components
- kubelet vs control plane
- kubectl vs cluster

**Solution Steps:**
1. Research version skew policy
2. Understand supported version differences
3. Plan upgrades accordingly

## Scenario 6.3: Backup and Restore

### Question 6.3.1: Backup etcd
Backup etcd data (for clusters using etcd).

**Solution Steps:**
1. For kubeadm clusters, etcd runs as static pod
2. Backup command (example):
   ```bash
   ETCDCTL_API=3 etcdctl snapshot save /backup/etcd-snapshot.db \
     --endpoints=https://127.0.0.1:2379 \
     --cacert=/etc/kubernetes/pki/etcd/ca.crt \
     --cert=/etc/kubernetes/pki/etcd/server.crt \
     --key=/etc/kubernetes/pki/etcd/server.key
   ```

### Question 6.3.2: Export Resources
Export all resources from a namespace.

**Solution Steps:**
1. Export all: `kubectl get all -n <namespace> -o yaml > backup.yaml`
2. Export specific resource: `kubectl get deployment <name> -o yaml > deployment.yaml`
3. Export with all resources: `kubectl get all,configmap,secret -n <namespace> -o yaml > full-backup.yaml`

### Question 6.3.3: Export Specific Resource Types
Export specific resource types from namespace:
- All ConfigMaps
- All Secrets
- All Services
- All Deployments

**Solution Steps:**
1. Export ConfigMaps: `kubectl get configmap -n <namespace> -o yaml > configmaps.yaml`
2. Export Secrets: `kubectl get secret -n <namespace> -o yaml > secrets.yaml`
3. Export Services: `kubectl get svc -n <namespace> -o yaml > services.yaml`
4. Export Deployments: `kubectl get deployment -n <namespace> -o yaml > deployments.yaml`

### Question 6.3.4: Backup with Labels
Export resources filtered by labels:
- Export all resources with label `app=web`
- Export all resources with label `env=production`

**Solution Steps:**
1. Export by label: `kubectl get all -l app=web -n <namespace> -o yaml > web-backup.yaml`
2. Export multiple labels: `kubectl get all -l app=web,env=prod -n <namespace> -o yaml`

### Question 6.3.5: Restore Resources
Restore resources from backup:
- Restore from exported YAML file
- Handle namespace conflicts

**Solution Steps:**
1. Review backup file
2. Apply: `kubectl apply -f backup.yaml`
3. Handle conflicts: `kubectl apply -f backup.yaml --force --grace-period=0` (if needed)

## Scenario 6.4: Resource Management

### Question 6.4.1: ResourceQuota
Create a ResourceQuota for a namespace:
- CPU requests: 2
- CPU limits: 4
- Memory requests: 4Gi
- Memory limits: 8Gi
- Pods: 10

**Solution Steps:**
1. Check `solutions/resourcequota-example.yaml`
2. Apply: `kubectl apply -f solutions/resourcequota-example.yaml`
3. Verify: `kubectl get resourcequota -n <namespace>`
4. Describe: `kubectl describe resourcequota -n <namespace>`

### Question 6.4.2: LimitRange
Create a LimitRange for a namespace that sets:
- Default CPU request: 100m
- Default CPU limit: 200m
- Default memory request: 128Mi
- Default memory limit: 256Mi
- Max CPU limit: 2
- Max memory limit: 4Gi

**Solution Steps:**
1. Check `solutions/limitrange-example.yaml`
2. Apply and verify
3. Create a pod without resources and verify defaults applied

### Question 6.4.3: ResourceQuota for Storage
Create a ResourceQuota that limits storage:
- Persistent volume claims: 10
- Total storage requests: 100Gi

**Solution Steps:**
1. Check `solutions/resourcequota-storage.yaml`
2. Apply and verify
3. Test by creating PVCs

### Question 6.4.4: ResourceQuota for Objects
Create a ResourceQuota that limits object counts:
- Pods: 20
- Services: 10
- ConfigMaps: 15
- Secrets: 10

**Solution Steps:**
1. Check `solutions/resourcequota-objects.yaml`
2. Apply and verify
3. Test by creating resources

### Question 6.4.5: LimitRange for Pods
Create a LimitRange that sets limits per pod:
- Max CPU per pod: 4
- Max memory per pod: 8Gi
- Min CPU per pod: 100m
- Min memory per pod: 128Mi

**Solution Steps:**
1. Check `solutions/limitrange-pod.yaml`
2. Apply and verify
3. Test by creating pods with different resource requests

### Question 6.4.6: LimitRange for PersistentVolumeClaims
Create a LimitRange for PVCs:
- Min storage: 1Gi
- Max storage: 50Gi
- Default storage request: 10Gi

**Solution Steps:**
1. Check `solutions/limitrange-pvc.yaml`
2. Apply and verify
3. Create PVC without size and verify default applied

## Scenario 6.5: Monitoring and Logging

### Question 6.5.1: Check Resource Usage
Check CPU and memory usage of nodes and pods.

**Solution Steps:**
1. Install metrics-server if not present
2. Node usage: `kubectl top nodes`
3. Pod usage: `kubectl top pods`
4. Pod usage with containers: `kubectl top pods --containers`

### Question 6.5.2: View Logs
View logs from various sources:
- Pod logs: `kubectl logs <pod-name>`
- Previous container: `kubectl logs <pod-name> --previous`
- All containers: `kubectl logs <pod-name> --all-containers=true`
- Follow logs: `kubectl logs -f <pod-name>`
- Since time: `kubectl logs <pod-name> --since=1h`

### Question 6.5.3: Logs from Multiple Pods
View logs from multiple pods:
- All pods with label `app=web`
- All pods in namespace
- Follow logs from multiple pods

**Solution Steps:**
1. Logs from labeled pods: `kubectl logs -l app=web`
2. Logs from all pods in namespace: `kubectl logs --all-namespaces -l app=web`
3. Follow: `kubectl logs -f -l app=web`

### Question 6.5.4: Logs with Timestamps
View logs with timestamps:
- Add timestamps to log output
- Filter logs by time range

**Solution Steps:**
1. With timestamps: `kubectl logs <pod-name> --timestamps`
2. Since time: `kubectl logs <pod-name> --since=30m`
3. Since specific time: `kubectl logs <pod-name> --since-time=2024-01-01T00:00:00Z`

### Question 6.5.5: Resource Usage Monitoring
Monitor resource usage over time:
- Watch node resource usage
- Watch pod resource usage
- Identify resource-hungry pods

**Solution Steps:**
1. Watch nodes: `watch kubectl top nodes`
2. Watch pods: `watch kubectl top pods`
3. Sort by CPU: `kubectl top pods --sort-by=cpu`
4. Sort by memory: `kubectl top pods --sort-by=memory`

### Question 6.5.6: Events Monitoring
Monitor cluster events:
- Watch events in real-time
- Filter events by type
- Filter events by object

**Solution Steps:**
1. Watch events: `kubectl get events -w`
2. Filter by type: `kubectl get events --field-selector type=Warning`
3. Filter by object: `kubectl get events --field-selector involvedObject.name=<pod-name>`
4. Recent events: `kubectl get events --sort-by=.lastTimestamp | tail -20`

## Scenario 6.6: Cluster Configuration

### Question 6.6.1: View Cluster Information
Get comprehensive cluster information:
- Cluster name
- API server endpoint
- Kubernetes version
- Number of nodes

**Solution Steps:**
1. Cluster info: `kubectl cluster-info`
2. Context: `kubectl config view`
3. Current context: `kubectl config current-context`
4. Nodes: `kubectl get nodes`

### Question 6.6.2: Switch Contexts
Switch between Kubernetes contexts:
- List all contexts
- Switch to specific context
- Verify current context

**Solution Steps:**
1. List: `kubectl config get-contexts`
2. Switch: `kubectl config use-context <context-name>`
3. Verify: `kubectl config current-context`

### Question 6.6.3: Set Default Namespace
Set default namespace for current context:
- Set namespace: `production`
- Verify all commands use this namespace
- Reset to default

**Solution Steps:**
1. Set: `kubectl config set-context --current --namespace=production`
2. Verify: `kubectl config view --minify | grep namespace`
3. Reset: `kubectl config set-context --current --namespace=default`

### Question 6.6.4: View API Resources
List all available API resources:
- All resources in cluster
- Resources in specific API group
- Resources with short names

**Solution Steps:**
1. All resources: `kubectl api-resources`
2. With verbs: `kubectl api-resources --verbs=list,get`
3. Specific group: `kubectl api-resources --api-group=apps`
4. With short names: `kubectl api-resources --output=wide`

## Scenario 6.7: Performance and Optimization

### Question 6.7.1: Identify Resource-Heavy Pods
Find pods consuming most resources:
- Sort by CPU usage
- Sort by memory usage
- Identify optimization opportunities

**Solution Steps:**
1. CPU: `kubectl top pods --sort-by=cpu --all-namespaces`
2. Memory: `kubectl top pods --sort-by=memory --all-namespaces`
3. Specific namespace: `kubectl top pods -n <namespace> --sort-by=cpu`

### Question 6.7.2: Node Resource Analysis
Analyze node resource utilization:
- Check allocatable vs capacity
- Identify underutilized nodes
- Plan resource optimization

**Solution Steps:**
1. Node resources: `kubectl describe node <node-name> | grep -A 10 "Allocated resources"`
2. Top nodes: `kubectl top nodes`
3. Check capacity: `kubectl get node <node-name> -o jsonpath='{.status.capacity}'`

### Question 6.7.3: Pod Resource Optimization
Optimize pod resource requests and limits:
- Analyze actual usage vs requests
- Adjust requests to match usage
- Set appropriate limits

**Solution Steps:**
1. Check usage: `kubectl top pod <pod-name>`
2. Check requests: `kubectl describe pod <pod-name> | grep -A 5 "Limits\|Requests"`
3. Update deployment with optimized resources
4. Verify improvements

## Scenario 6.8: Disaster Recovery

### Question 6.8.1: Backup Critical Resources
Backup critical cluster resources:
- All namespaces
- All ConfigMaps and Secrets
- All PersistentVolumes
- Cluster-level resources

**Solution Steps:**
1. Namespaces: `kubectl get namespaces -o yaml > namespaces-backup.yaml`
2. All resources: `kubectl get all --all-namespaces -o yaml > all-resources.yaml`
3. ConfigMaps: `kubectl get configmap --all-namespaces -o yaml > configmaps-backup.yaml`
4. Secrets: `kubectl get secret --all-namespaces -o yaml > secrets-backup.yaml`

### Question 6.8.2: Restore from Backup
Restore resources from backup:
- Restore specific namespace
- Handle resource conflicts
- Verify restoration

**Solution Steps:**
1. Review backup file
2. Apply: `kubectl apply -f backup.yaml`
3. Handle conflicts if needed
4. Verify: `kubectl get all -n <namespace>`

### Question 6.8.3: etcd Backup Verification
Verify etcd backup integrity:
- Check backup file exists
- Verify backup size
- Test restore (if possible)

**Solution Steps:**
1. List backups: `ls -lh /backup/etcd-*.db`
2. Check size: `du -h /backup/etcd-snapshot.db`
3. Verify backup (if etcdctl available): `etcdctl snapshot status /backup/etcd-snapshot.db`

## Scenario 6.9: Cluster Health Checks

### Question 6.9.1: Cluster Component Health
Check health of cluster components:
- API server status
- etcd status
- Controller manager
- Scheduler

**Solution Steps:**
1. API server: `kubectl cluster-info`
2. Component status: `kubectl get componentstatuses` (deprecated, use `kubectl get cs`)
3. Check pods: `kubectl get pods -n kube-system`

### Question 6.9.2: Node Health Check
Perform comprehensive node health check:
- Node conditions
- Kubelet status
- Container runtime status
- Network connectivity

**Solution Steps:**
1. Conditions: `kubectl describe node <node-name> | grep -A 10 Conditions`
2. Kubelet: Check kubelet service on node
3. Runtime: `kubectl get node <node-name> -o jsonpath='{.status.nodeInfo.containerRuntimeVersion}'`
4. Network: Test pod-to-pod connectivity

### Question 6.9.3: Resource Availability Check
Check resource availability:
- CPU availability across nodes
- Memory availability
- Storage availability
- Pod capacity

**Solution Steps:**
1. Node resources: `kubectl describe nodes | grep -A 5 "Allocated resources"`
2. Storage: `kubectl get pv`
3. Pod capacity: `kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.allocatable.pods}{"\n"}{end}'`

## Practice Commands

```bash
# Node management
kubectl get nodes
kubectl cordon <node-name>
kubectl uncordon <node-name>
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data
kubectl label nodes <node-name> <key>=<value>
kubectl taint nodes <node-name> <key>=<value>:<effect>

# Version information
kubectl version
kubectl get nodes -o wide

# Resource quotas
kubectl get resourcequota
kubectl describe resourcequota <name> -n <namespace>

# Limit ranges
kubectl get limitrange
kubectl describe limitrange <name> -n <namespace>

# Resource usage
kubectl top nodes
kubectl top pods
kubectl top pods --containers

# Logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl logs -f <pod-name> --all-containers
kubectl logs -l app=<label>
kubectl logs <pod-name> --timestamps
kubectl logs <pod-name> --since=1h
kubectl logs <pod-name> --since-time=2024-01-01T00:00:00Z

# Events
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get events --field-selector type=Warning
kubectl get events --field-selector involvedObject.name=<pod-name>

# Resource monitoring
watch kubectl top nodes
watch kubectl top pods
kubectl top pods --sort-by=cpu
kubectl top pods --sort-by=memory
```

