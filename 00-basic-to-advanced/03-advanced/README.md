# Level 3: Advanced - Complex Scenarios

Master complex Kubernetes scenarios and expert-level techniques.

## Topic 1: Advanced Pod Configuration

### Question 3.1: Pod Security Context
**Task**: Configure comprehensive pod security.

**Steps**:
1. Create pod running as non-root user
2. Configure read-only root filesystem
3. Drop all capabilities, add specific ones
4. Set SELinux options
5. Configure seccomp profile
6. Test pod functionality

**Solution**:
See `solutions/pod-security-context.yaml`

**Expected Time**: 8 minutes

---

### Question 3.2: Pod with Node Affinity
**Task**: Control pod scheduling with node affinity.

**Steps**:
1. Label nodes with different zones
2. Create pod with required node affinity
3. Create pod with preferred node affinity
4. Use different operators (In, NotIn, Exists)
5. Test pod placement

**Solution**:
See `solutions/pod-node-affinity.yaml`

**Expected Time**: 8 minutes

---

### Question 3.3: Pod with Tolerations
**Task**: Schedule pods on tainted nodes.

**Steps**:
1. Taint a node with NoSchedule
2. Create pod with matching toleration
3. Test pod schedules on tainted node
4. Use TolerationSeconds for NoExecute
5. Understand taint effects

**Solution**:
See `solutions/pod-tolerations.yaml`

**Expected Time**: 7 minutes

---

### Question 3.4: Pod Disruption Budget
**Task**: Ensure pod availability during disruptions.

**Steps**:
1. Create deployment with 5 replicas
2. Create PDB with minAvailable 3
3. Try to drain a node
4. Observe PDB behavior
5. Create PDB with maxUnavailable

**Solution**:
See `solutions/pdb-deployment.yaml`

**Expected Time**: 8 minutes

---

## Topic 2: Advanced Deployments

### Question 3.5: Deployment with Pod Anti-Affinity
**Task**: Distribute pods across nodes.

**Steps**:
1. Create deployment with pod anti-affinity
2. Ensure pods on different nodes
3. Use required vs preferred
4. Test with topology keys
5. Verify distribution

**Solution**:
See `solutions/deployment-antiaffinity.yaml`

**Expected Time**: 8 minutes

---

### Question 3.6: Deployment with Pod Affinity
**Task**: Co-locate pods on same node.

**Steps**:
1. Create cache deployment
2. Create app deployment with pod affinity to cache
3. Verify pods are co-located
4. Use topology keys
5. Understand use cases

**Solution**:
See `solutions/deployment-affinity.yaml`

**Expected Time**: 8 minutes

---

### Question 3.7: Canary Deployment
**Task**: Implement canary deployment pattern.

**Steps**:
1. Create stable deployment (5 replicas)
2. Create canary deployment (1 replica)
3. Use same labels for service
4. Monitor canary performance
5. Rollout or rollback based on metrics

**Solution**:
See `solutions/canary-deployment.yaml`

**Expected Time**: 10 minutes

---

## Topic 3: StatefulSets

### Question 3.8: Basic StatefulSet
**Task**: Create and understand StatefulSets.

**Steps**:
1. Create headless service
2. Create StatefulSet with 3 replicas
3. Observe ordered pod creation
4. Check stable network identities
5. Test DNS resolution

**Solution**:
See `solutions/statefulset-basic.yaml`

**Expected Time**: 8 minutes

---

### Question 3.9: StatefulSet with Storage
**Task**: StatefulSet with persistent storage.

**Steps**:
1. Create StorageClass
2. Create StatefulSet with volumeClaimTemplates
3. Verify each pod gets its own PVC
4. Delete a pod and verify PVC reuse
5. Scale StatefulSet and observe storage

**Solution**:
See `solutions/statefulset-storage.yaml`

**Expected Time**: 10 minutes

---

### Question 3.10: StatefulSet Update Strategy
**Task**: Control StatefulSet updates.

**Steps**:
1. Create StatefulSet with 5 replicas
2. Configure partition for canary update
3. Update image and verify only some pods update
4. Reduce partition to update remaining
5. Understand ordered updates

**Solution**:
```bash
# Create StatefulSet
kubectl apply -f solutions/statefulset-basic.yaml

# Set partition
kubectl patch statefulset web-statefulset -p '{"spec":{"updateStrategy":{"type":"RollingUpdate","rollingUpdate":{"partition":3}}}}'

# Update image
kubectl set image statefulset/web-statefulset nginx=nginx:1.22

# Verify only 0,1,2 update
kubectl get pods -l app=web

# Update remaining
kubectl patch statefulset web-statefulset -p '{"spec":{"updateStrategy":{"type":"RollingUpdate","rollingUpdate":{"partition":0}}}'
```

**Expected Time**: 10 minutes

---

## Topic 4: Jobs and CronJobs

### Question 3.11: Basic Job
**Task**: Create and manage Jobs.

**Steps**:
1. Create a Job that completes successfully
2. Create a Job with multiple completions
3. Create a parallel Job
4. Check Job status and logs
5. Understand Job vs Pod

**Solution**:
See `solutions/job-basic.yaml`

**Expected Time**: 6 minutes

---

### Question 3.12: Job with Retries
**Task**: Configure Job retry behavior.

**Steps**:
1. Create Job with backoffLimit
2. Create Job that will fail
3. Observe retry behavior
4. Create Job with activeDeadlineSeconds
5. Create Job with TTL

**Solution**:
See `solutions/job-retry.yaml`

**Expected Time**: 7 minutes

---

### Question 3.13: CronJob
**Task**: Create scheduled Jobs.

**Steps**:
1. Create CronJob with daily schedule
2. Create CronJob with concurrency policy
3. Suspend and resume CronJob
4. Manually trigger CronJob
5. Check Job history

**Solution**:
See `solutions/cronjob-basic.yaml`

**Expected Time**: 7 minutes

---

## Topic 5: DaemonSets

### Question 3.14: Basic DaemonSet
**Task**: Create DaemonSet for all nodes.

**Steps**:
1. Create DaemonSet that runs on all nodes
2. Verify one pod per node
3. Add a new node and observe
4. Update DaemonSet image
5. Understand update strategy

**Solution**:
See `solutions/daemonset-basic.yaml`

**Expected Time**: 7 minutes

---

### Question 3.15: DaemonSet with Node Selector
**Task**: DaemonSet on specific nodes.

**Steps**:
1. Label nodes
2. Create DaemonSet with node selector
3. Verify only runs on labeled nodes
4. Create DaemonSet with tolerations
5. Test on tainted nodes

**Solution**:
See `solutions/daemonset-selector.yaml`

**Expected Time**: 8 minutes

---

## Topic 6: Advanced Networking

### Question 3.16: Ingress Configuration
**Task**: Configure Ingress with multiple rules.

**Steps**:
1. Create Ingress with single host
2. Create Ingress with multiple paths
3. Create Ingress with TLS
4. Configure default backend
5. Test routing

**Solution**:
See `solutions/ingress-advanced.yaml`

**Expected Time**: 10 minutes

---

### Question 3.17: NetworkPolicy
**Task**: Implement network isolation.

**Steps**:
1. Create default deny NetworkPolicy
2. Allow specific ingress traffic
3. Configure egress rules
4. Use IP blocks
5. Test connectivity

**Solution**:
See `solutions/networkpolicy-complete.yaml`

**Expected Time**: 12 minutes

---

## Topic 7: Advanced Storage

### Question 3.18: Multiple Volume Types
**Task**: Use different volume types.

**Steps**:
1. Create pod with emptyDir
2. Create pod with ConfigMap volume
3. Create pod with Secret volume
4. Create pod with projected volume
5. Create pod with downward API

**Solution**:
See `solutions/pod-volumes.yaml`

**Expected Time**: 10 minutes

---

### Question 3.19: PVC Expansion
**Task**: Expand persistent volumes.

**Steps**:
1. Create StorageClass with allowVolumeExpansion
2. Create PVC
3. Expand PVC
4. Verify expansion
5. Understand limitations

**Solution**:
```bash
# Create StorageClass with expansion
kubectl apply -f solutions/storageclass-expandable.yaml

# Create PVC
kubectl apply -f solutions/pvc-small.yaml

# Expand
kubectl patch pvc my-pvc -p '{"spec":{"resources":{"requests":{"storage":"20Gi"}}}}'

# Verify
kubectl get pvc my-pvc
```

**Expected Time**: 8 minutes

---

## Topic 8: Advanced RBAC

### Question 3.20: ClusterRole and ClusterRoleBinding
**Task**: Configure cluster-wide permissions.

**Steps**:
1. Create ClusterRole for nodes
2. Create ClusterRoleBinding
3. Test cluster-wide access
4. Compare with Role
5. Understand use cases

**Solution**:
See `solutions/clusterrole-basic.yaml`

**Expected Time**: 8 minutes

---

### Question 3.21: RBAC with Resource Names
**Task**: Restrict access to specific resources.

**Steps**:
1. Create Role with resourceNames
2. Test access to named resource
3. Test access to other resources
4. Understand use cases
5. Combine with other rules

**Solution**:
See `solutions/role-resourcenames.yaml`

**Expected Time**: 7 minutes

---

## Topic 9: Advanced Troubleshooting

### Question 3.22: Multi-Issue Troubleshooting
**Task**: Fix multiple related issues.

**Steps**:
1. Diagnose node NotReady
2. Fix pod scheduling issues
3. Resolve service connectivity
4. Fix storage problems
5. Verify all fixes

**Solution**:
See `solutions/troubleshooting-multi.yaml`

**Expected Time**: 15 minutes

---

### Question 3.23: Performance Issues
**Task**: Identify and fix performance problems.

**Steps**:
1. Identify resource-hungry pods
2. Check node resource usage
3. Optimize resource requests
4. Check for resource leaks
5. Verify improvements

**Solution**:
```bash
# Find resource usage
kubectl top pods --all-namespaces --sort-by=cpu
kubectl top pods --all-namespaces --sort-by=memory

# Check node capacity
kubectl describe nodes | grep -A 10 "Allocated resources"

# Optimize
kubectl edit deployment <name>
# Adjust resource requests/limits
```

**Expected Time**: 12 minutes

---

## Practice Exercises

### Exercise 1: Complete Advanced Setup
Deploy advanced application:
1. StatefulSet with persistent storage
2. Deployment with anti-affinity
3. Service with session affinity
4. Ingress with TLS
5. NetworkPolicy for isolation
6. RBAC with least privilege
7. ResourceQuota and LimitRange

**Expected Time**: 30 minutes

### Exercise 2: Disaster Recovery
Simulate disaster recovery:
1. Backup all resources
2. Delete namespace
3. Restore from backup
4. Verify functionality
5. Document process

**Expected Time**: 20 minutes

---

## Topic 10: Resource Management

### Question 3.24: ResourceQuota Configuration
**Task**: Implement comprehensive resource quotas.

**Steps**:
1. Create ResourceQuota for CPU and Memory
2. Create ResourceQuota for object counts
3. Create ResourceQuota for storage
4. Test quota enforcement
5. Understand quota scopes

**Solution**:
See `solutions/resourcequota-complete.yaml`

**Expected Time**: 8 minutes

---

### Question 3.25: LimitRange Configuration
**Task**: Set default limits for namespace.

**Steps**:
1. Create LimitRange for containers
2. Create LimitRange for pods
3. Create LimitRange for PVCs
4. Test default application
5. Verify max/min constraints

**Solution**:
See `solutions/limitrange-complete.yaml`

**Expected Time**: 8 minutes

---

## Topic 11: Advanced Networking Patterns

### Question 3.26: Service Mesh Pattern
**Task**: Implement service mesh-like networking.

**Steps**:
1. Create multiple services
2. Configure NetworkPolicies for service chain
3. Implement traffic rules
4. Test service-to-service communication
5. Verify isolation

**Solution**:
See `solutions/networkpolicy-mesh.yaml`

**Expected Time**: 15 minutes

---

### Question 3.27: Cross-Namespace Communication
**Task**: Enable secure cross-namespace access.

**Steps**:
1. Create services in different namespaces
2. Configure NetworkPolicies
3. Test FQDN resolution
4. Verify connectivity
5. Implement security rules

**Solution**:
See `solutions/cross-namespace-networking.yaml`

**Expected Time**: 12 minutes

---

## Topic 12: Advanced Storage Patterns

### Question 3.28: StatefulSet with Multiple Volumes
**Task**: StatefulSet with multiple volume claims.

**Steps**:
1. Create StatefulSet with 2 volumeClaimTemplates
2. Verify each pod gets both volumes
3. Test data persistence
4. Scale and verify volumes
5. Understand volume naming

**Solution**:
See `solutions/statefulset-multi-volume.yaml`

**Expected Time**: 10 minutes

---

### Question 3.29: Volume Snapshots
**Task**: Create and restore volume snapshots.

**Steps**:
1. Create VolumeSnapshotClass
2. Create VolumeSnapshot
3. Verify snapshot
4. Restore from snapshot
5. Use restored volume

**Solution**:
See `solutions/volumesnapshot.yaml` (if supported)

**Expected Time**: 12 minutes

---

## Topic 13: Advanced Security

### Question 3.30: Pod Security Standards
**Task**: Implement Pod Security Standards.

**Steps**:
1. Apply baseline standard to namespace
2. Apply restricted standard
3. Create pods that comply
4. Test enforcement
5. Understand exemptions

**Solution**:
See `solutions/pod-security-standards.yaml`

**Expected Time**: 10 minutes

---

### Question 3.31: RBAC Aggregation
**Task**: Use ClusterRole aggregation.

**Steps**:
1. Create ClusterRole with aggregation labels
2. Create component ClusterRoles
3. Verify aggregated permissions
4. Test access
5. Understand aggregation rules

**Solution**:
See `solutions/clusterrole-aggregation.yaml`

**Expected Time**: 10 minutes

---

## Topic 14: Advanced Troubleshooting

### Question 3.32: Performance Bottleneck
**Task**: Identify and fix performance issues.

**Steps**:
1. Identify slow pods
2. Check resource usage
3. Analyze bottlenecks
4. Optimize configuration
5. Verify improvements

**Solution**:
```bash
# Identify issues
kubectl top pods --all-namespaces --sort-by=cpu
kubectl top pods --all-namespaces --sort-by=memory
kubectl describe nodes | grep -A 10 "Allocated resources"

# Check for issues
kubectl get events --sort-by=.lastTimestamp
kubectl get pods --field-selector=status.phase!=Running

# Optimize
kubectl edit deployment <name>
# Adjust resources, probes, etc.
```

**Expected Time**: 15 minutes

---

### Question 3.33: Network Connectivity Issues
**Task**: Diagnose complex networking problems.

**Steps**:
1. Test pod-to-pod connectivity
2. Test pod-to-service connectivity
3. Check DNS resolution
4. Verify NetworkPolicies
5. Test cross-namespace
6. Fix all issues

**Solution**:
See `solutions/troubleshooting-network.yaml`

**Expected Time**: 18 minutes

---

## Topic 15: Cluster Operations

### Question 3.34: Node Maintenance Workflow
**Task**: Complete node maintenance procedure.

**Steps**:
1. Identify node for maintenance
2. Cordon the node
3. Drain gracefully
4. Perform maintenance (simulated)
5. Uncordon node
6. Verify cluster health

**Solution**:
```bash
# Cordon
kubectl cordon <node-name>

# Drain
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data

# Maintenance (simulated)
# ...

# Uncordon
kubectl uncordon <node-name>

# Verify
kubectl get nodes
kubectl get pods --all-namespaces
```

**Expected Time**: 10 minutes

---

### Question 3.35: Cluster Backup and Restore
**Task**: Complete backup and restore procedure.

**Steps**:
1. Backup etcd
2. Export all resources
3. Document restore procedure
4. Test restore (if possible)
5. Verify backup integrity

**Solution**:
See `solutions/backup-restore.yaml`

**Expected Time**: 15 minutes

---

## Practice Exercises

### Exercise 1: Production Deployment
Deploy production-ready application:
- All advanced features
- Security hardened
- High availability
- Monitoring ready
- Backup configured

**Expected Time**: 45 minutes

### Exercise 2: Incident Response
Handle production incident:
- Multiple related issues
- Performance degradation
- Security breach simulation
- Quick resolution
- Documentation

**Expected Time**: 40 minutes

---

## Solutions

All solutions in `solutions/` folder.

## Next Level

Ready for expert level? Proceed to:
- **04-expert/** - Mastery level challenges

