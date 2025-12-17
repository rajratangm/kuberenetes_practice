# CKA Exam Official Curriculum Breakdown

## Exam Overview
- **Duration**: 2 hours
- **Format**: Performance-based (hands-on tasks)
- **Passing Score**: 74% (66% for CKAD)
- **Total Questions**: 15-20 tasks
- **Environment**: Live Kubernetes cluster via PSI Bridge

## Official Curriculum Domains & Weights

### 1. Cluster Architecture, Installation & Configuration (25%)
**Weight**: ~25% of exam

**Topics Covered**:
- Manage role based access control (RBAC)
- Use Kubeadm to install a basic cluster
- Manage a highly-available Kubernetes cluster
- Provision underlying infrastructure to deploy a Kubernetes cluster
- Perform a version upgrade on a Kubernetes cluster using Kubeadm
- Implement etcd backup and restore

**Key Skills**:
- Understanding cluster components (API server, etcd, kubelet, controller manager, scheduler)
- Configuring and managing cluster nodes
- Cluster upgrade procedures
- etcd operations (backup, restore, health checks)
- RBAC configuration (Roles, RoleBindings, ClusterRoles, ClusterRoleBindings)
- Service accounts and token management

**Common Exam Tasks**:
- Create/update RBAC resources
- Backup and restore etcd
- Upgrade cluster version
- Troubleshoot cluster component issues
- Configure kubelet on nodes

---

### 2. Workloads & Scheduling (15%)
**Weight**: ~15% of exam

**Topics Covered**:
- Understand deployments and how to perform rolling updates and rollbacks
- Understand ConfigMaps
- Understand Secrets
- Understand how to scale applications
- Understand the primitives used to create robust, self-healing, application deployments
- Understand how resource limits can affect Pod scheduling
- Awareness of manifest management and common templating tools

**Key Skills**:
- Creating and managing Deployments, ReplicaSets, DaemonSets, StatefulSets
- Configuring rolling updates and rollbacks
- Working with ConfigMaps and Secrets
- Understanding Pod scheduling (node selectors, affinity, taints/tolerations)
- Resource requests and limits
- Jobs and CronJobs

**Common Exam Tasks**:
- Create/update Deployments with specific configurations
- Perform rolling updates and rollbacks
- Create ConfigMaps and mount them in Pods
- Create Secrets and use them securely
- Scale applications
- Configure resource limits
- Create Jobs and CronJobs

---

### 3. Services & Networking (20%)
**Weight**: ~20% of exam

**Topics Covered**:
- Understand host networking configuration on the cluster nodes
- Understand connectivity between Pods
- Understand ClusterIP, NodePort, LoadBalancer and Ingress services and use cases
- Understand how to use Ingress controllers and Ingress resources
- Understand how to configure and use CoreDNS
- Choose an appropriate container network interface plugin

**Key Skills**:
- Creating and configuring Services (ClusterIP, NodePort, LoadBalancer)
- Configuring Ingress resources and controllers
- Understanding Pod networking
- DNS configuration (CoreDNS)
- Network policies
- Service discovery

**Common Exam Tasks**:
- Create Services with specific types and configurations
- Configure Ingress resources
- Troubleshoot networking issues
- Configure NetworkPolicies
- Verify DNS resolution
- Expose applications via different service types

---

### 4. Storage (10%)
**Weight**: ~10% of exam

**Topics Covered**:
- Understand storage classes, persistent volumes
- Understand volume modes, access modes and reclaim policies for volumes
- Understand persistent volume claims primitive
- Understand how to configure applications with persistent storage

**Key Skills**:
- Creating and managing PersistentVolumes (PV)
- Creating and managing PersistentVolumeClaims (PVC)
- Understanding StorageClasses
- Volume access modes (RWO, ROX, RWX)
- Volume reclaim policies (Retain, Recycle, Delete)
- Mounting volumes in Pods
- Understanding volume types (emptyDir, hostPath, configMap, secret, etc.)

**Common Exam Tasks**:
- Create PVs and PVCs
- Configure StorageClasses
- Mount persistent storage in Pods
- Troubleshoot storage issues
- Configure volume access modes and reclaim policies

---

### 5. Troubleshooting (30%)
**Weight**: ~30% of exam (LARGEST DOMAIN)

**Topics Covered**:
- Evaluate cluster and node logging
- Understand how to monitor applications
- Manage container stdout & stderr logs
- Troubleshoot application failures
- Troubleshoot cluster component failures
- Troubleshoot networking

**Key Skills**:
- Using kubectl logs, describe, get events
- Debugging Pod issues (CrashLoopBackOff, ImagePullBackOff, etc.)
- Troubleshooting Service connectivity
- Debugging Deployment issues
- Analyzing cluster component logs
- Network troubleshooting
- Resource constraint issues
- Configuration errors

**Common Exam Tasks**:
- Diagnose and fix Pod failures
- Troubleshoot Service connectivity
- Fix Deployment issues
- Debug cluster component problems
- Analyze logs and events
- Resolve resource constraint issues
- Fix configuration errors

---

## Exam Format & Question Types

### Typical Question Structure
1. **Context**: Brief scenario description
2. **Task**: Specific action to perform
3. **Requirements**: Exact specifications
4. **Verification**: How to confirm success

### Question Characteristics
- **Performance-based**: All tasks require hands-on work
- **Time-boxed**: Average 6-8 minutes per task
- **Verification required**: Must verify your work
- **Namespace awareness**: Often work in specific namespaces
- **Real-world scenarios**: Practical problems you'd face as an admin

### Common Task Patterns
1. **Create resources** with specific configurations
2. **Troubleshoot** existing broken resources
3. **Update** existing resources to meet new requirements
4. **Verify** configurations and connectivity
5. **Backup/Restore** operations
6. **Scale** applications
7. **Configure** networking and security

---

## Study Strategy by Domain

### High Priority (Must Master)
1. **Troubleshooting (30%)** - Largest domain, focus heavily here
2. **Cluster Architecture (25%)** - Core administrative skills
3. **Services & Networking (20%)** - Critical for production

### Medium Priority
4. **Workloads & Scheduling (15%)** - Foundation knowledge
5. **Storage (10%)** - Smaller but still important

### Recommended Study Order
1. Start with **Workloads & Scheduling** (foundation)
2. Move to **Storage** (simpler concepts)
3. Deep dive into **Services & Networking**
4. Master **Cluster Architecture**
5. Practice **Troubleshooting** extensively (use all other domains)

---

## Key Commands to Master

### Essential kubectl Commands
```bash
# Resource management
kubectl get/describe/create/delete/edit/apply

# Troubleshooting
kubectl logs
kubectl describe
kubectl get events
kubectl exec

# Configuration
kubectl explain
kubectl api-resources
kubectl config

# Debugging
kubectl top
kubectl debug
kubectl port-forward
```

### etcd Commands
```bash
# Backup
ETCDCTL_API=3 etcdctl snapshot save

# Restore
ETCDCTL_API=3 etcdctl snapshot restore
```

### kubeadm Commands
```bash
kubeadm init
kubeadm join
kubeadm upgrade
kubeadm config
```

---

## Exam Day Tips

1. **Time Management**
   - Quick wins first (5-8 min tasks)
   - Medium tasks next (10-12 min)
   - Complex tasks last (15-20 min)
   - Leave 10-15 min for review

2. **Verification**
   - Always verify after each task
   - Use `kubectl get` to check resources
   - Check events for errors
   - Test connectivity when applicable

3. **Common Mistakes to Avoid**
   - Wrong namespace
   - Selector mismatches
   - Missing required fields
   - Typos in resource names
   - Forgetting to verify

4. **Allowed Resources**
   - Kubernetes official documentation
   - `kubectl explain` command
   - kubectl help

5. **Not Allowed**
   - External websites
   - Chat/communication tools
   - Copy-paste from external sources

---

## Practice Exam Recommendations

Based on this curriculum, practice exams should:
- **30%** Troubleshooting scenarios
- **25%** Cluster Architecture tasks
- **20%** Services & Networking
- **15%** Workloads & Scheduling
- **10%** Storage

Each exam should have 15-20 tasks covering all domains proportionally.

---

## Additional Resources

- **Official CKA Curriculum**: https://www.cncf.io/certification/cka/
- **Kubernetes Documentation**: https://kubernetes.io/docs/
- **kubectl Cheat Sheet**: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
- **etcd Documentation**: https://etcd.io/docs/

---

**Last Updated**: Based on 2024 CKA Exam Curriculum
**Next Review**: Check CNCF website for curriculum updates

