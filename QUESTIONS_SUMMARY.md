# CKA Practice Questions Summary

This document provides a comprehensive summary of all practice questions organized by topic.

## Total Questions by Category

### 01-Core Concepts: 20+ Questions
- Pod creation and management (10 questions)
- Deployments and ReplicaSets (10 questions)
- Pod lifecycle and states (2 questions)

**Key Topics Covered:**
- Basic pods with resources, labels, ports
- Multi-container pods
- Pods with ConfigMaps and Secrets
- Health probes (readiness, liveness, startup)
- Security contexts
- Node selectors and tolerations
- Init containers
- Lifecycle hooks
- Deployments with rolling updates
- Deployment rollbacks
- Pod disruption budgets
- Pod affinity and anti-affinity
- Scaling operations

### 02-Networking: 18+ Questions
- Services (9 questions)
- Ingress (4 questions)
- NetworkPolicies (5 questions)
- DNS and Service Discovery (4 questions)

**Key Topics Covered:**
- ClusterIP, NodePort, LoadBalancer services
- Headless services
- ExternalName services
- Services with multiple ports
- Session affinity
- Service endpoints
- Basic and advanced Ingress
- Ingress with TLS
- Ingress annotations and rewrites
- NetworkPolicy ingress/egress rules
- NetworkPolicy with IP blocks
- DNS resolution patterns
- Cross-namespace service discovery

### 03-Storage: 15+ Questions
- PersistentVolumes and PVCs (8 questions)
- StorageClasses (3 questions)
- Volume Types (4 questions)
- StatefulSets with Storage (3 questions)

**Key Topics Covered:**
- Static and dynamic provisioning
- PV reclaim policies
- PVC expansion
- StorageClass parameters
- emptyDir, ConfigMap, Secret volumes
- Projected volumes
- Downward API
- Volume mount propagation
- StatefulSet volume management
- Multiple volume claims

### 04-Security: 17+ Questions
- ServiceAccounts (4 questions)
- RBAC (10 questions)
- Secrets (7 questions)
- Pod Security Standards (5 questions)

**Key Topics Covered:**
- ServiceAccount creation and usage
- Image pull secrets
- Roles and RoleBindings
- ClusterRoles and ClusterRoleBindings
- Resource names and subresources
- User and group bindings
- Secret types and creation methods
- Secret rotation
- Pod security contexts
- Pod Security Standards (baseline, restricted)
- Comprehensive security configurations

### 05-Troubleshooting: 20+ Questions
- Pod Issues (7 questions)
- Service Issues (5 questions)
- Deployment Issues (5 questions)
- Node Issues (5 questions)
- Network Issues (2 questions)
- Storage Issues (4 questions)
- RBAC Issues (2 questions)
- Resource Quota Issues (1 question)

**Key Topics Covered:**
- Pod states (Pending, CrashLoopBackOff, ImagePullBackOff)
- OOMKilled pods
- Probe failures
- Service routing problems
- Deployment rollout issues
- Node resource pressure
- NetworkPolicy blocking
- PVC binding issues
- Permission denied errors
- Quota exceeded errors

### 06-Cluster Maintenance: 25+ Questions
- Node Management (6 questions)
- Cluster Upgrades (4 questions)
- Backup and Restore (5 questions)
- Resource Management (6 questions)
- Monitoring and Logging (6 questions)
- Cluster Configuration (4 questions)
- Performance and Optimization (3 questions)
- Disaster Recovery (3 questions)
- Cluster Health Checks (3 questions)

**Key Topics Covered:**
- Node cordon, drain, uncordon
- Node labels and taints management
- Version checking and upgrade process
- etcd backup and restore
- Resource export and import
- ResourceQuotas (compute, storage, objects)
- LimitRanges (containers, pods, PVCs)
- Resource usage monitoring
- Log management and filtering
- Events monitoring
- Context switching
- API resource discovery
- Performance analysis
- Disaster recovery procedures
- Node Management (2 questions)
- Cluster Upgrades (2 questions)
- Backup and Restore (2 questions)
- Resource Management (2 questions)
- Monitoring and Logging (2 questions)

**Key Topics Covered:**
- Node cordon, drain, uncordon
- Node labels and taints
- Version checking
- etcd backup
- Resource export
- ResourceQuotas
- LimitRanges
- Resource usage monitoring

### 07-Advanced Workloads: 10+ Questions
### 07-Advanced Workloads: 20+ Questions
- Jobs (6 questions)
- CronJobs (5 questions)
- DaemonSets (5 questions)
- StatefulSets (5 questions)
- Init Containers (4 questions)
- Complex Multi-Container Scenarios (3 questions)
- Workload Integration (3 questions)

**Key Topics Covered:**
- Basic and parallel jobs
- Job retries, deadlines, TTL
- Indexed completion mode
- CronJob scheduling expressions
- CronJob concurrency policies
- CronJob suspend and deadlines
- DaemonSet on all nodes
- DaemonSet update strategies
- DaemonSet with tolerations
- StatefulSet basics and scaling
- StatefulSet update strategies
- StatefulSet pod management
- Init containers with shared volumes
- Init container resource limits
- Sidecar, ambassador, adapter patterns
- Job triggered by CronJob
- DaemonSet with StatefulSet integration

**Key Topics Covered:**
- Basic and parallel jobs
- Job retries and backoff
- CronJob scheduling
- Concurrency policies
- DaemonSet on all nodes
- DaemonSet with node selectors
- StatefulSet basics
- StatefulSet with storage
- Init container patterns

### 08-Configuration: 20+ Questions
- ConfigMaps (6 questions)
- Secrets (4 questions)
- Resource Quotas and Limits (covered in maintenance)
- Pod Disruption Budgets (4 questions)
- Affinity and Anti-Affinity (6 questions)
- Taints and Tolerations (3 questions)
- Advanced Configuration Patterns (5 questions)
- Advanced Affinity Scenarios (3 questions)
- Resource Management Integration (3 questions)

**Key Topics Covered:**
- ConfigMap creation methods (literal, file, directory)
- ConfigMap with envFrom and optional keys
- ConfigMap hot reload and immutable ConfigMaps
- Secret types and creation methods
- Secret with envFrom
- PDB with min available and max unavailable
- PDB with complex selectors
- Pod affinity with topology keys
- Node affinity with operators (In, NotIn, Exists, etc.)
- Combined affinity rules
- Weighted affinity
- Toleration with TolerationSeconds
- Priority classes
- Immutable resources
- ConfigMaps (3 questions)
- Secrets (2 questions)
- Resource Quotas and Limits (covered in maintenance)
- Pod Disruption Budgets (2 questions)
- Affinity and Anti-Affinity (3 questions)
- Taints and Tolerations (1 question)

**Key Topics Covered:**
- ConfigMap creation methods
- ConfigMap in pods (env vars, volumes)
- Secret management
- PDB min available and max unavailable
- Pod affinity and anti-affinity
- Node affinity
- Tolerations

## Practice Exams and Integration Scenarios

**8 Complete Practice Exam Scenarios** covering:
1. Complete Application Setup
2. Networking and Ingress
3. Storage and StatefulSets
4. Security and RBAC
5. Troubleshooting
6. Advanced Workloads
7. Cluster Maintenance
8. Complete Multi-Tier Application

**8 Integration Scenarios** covering complex multi-step problems:
1. Complete Web Application Stack
2. Microservices with Service Mesh Concepts
3. Batch Processing System
4. Multi-Namespace Application
5. Disaster Recovery Setup
6. High-Performance Computing
7. Canary Deployment
8. Multi-Region Deployment

## Total Count

- **180+ Individual Practice Questions** (organized by topic)
- **50+ Scenario-Based Questions** (real exam-style, time-pressured)
- **8 Complete Practice Exam Scenarios**
- **8 Integration Scenarios** (complex multi-step problems)
- **2 Full Exam Simulations** (2-hour practice tests)
- **150+ Solution YAML Files**
- **Comprehensive Coverage of All CKA Exam Domains**

## Scenario-Based Questions Breakdown

### Basic Scenarios (5 questions)
- Simple pod deployment issues
- Service accessibility problems
- Deployment scaling issues
- ConfigMap/Secret access
- **Time**: 3-6 minutes each

### Intermediate Scenarios (7 questions)
- Rolling update stuck
- Persistent volume binding
- NetworkPolicy configuration
- RBAC permission issues
- Ingress routing
- StatefulSet pod issues
- Job completion problems
- **Time**: 6-9 minutes each

### Advanced Scenarios (18 questions)
- Multi-namespace service discovery
- Cluster resource exhaustion
- etcd backup and restore
- Node maintenance
- Deployment rollback
- Pod security policy
- Service mesh configuration
- Multi-container coordination
- Resource quota management
- And more...
- **Time**: 8-15 minutes each

### Real Exam-Style Scenarios (20 questions)
- Quick fixes (3-7 minutes)
- Time-pressured scenarios
- Multiple issues prioritization
- Complete application setup
- Troubleshooting marathon
- **Time**: 3-25 minutes each

### Exam Simulations (2 full exams)
- Complete 2-hour practice tests
- All exam domains covered
- Realistic time pressure
- Comprehensive evaluation

## Exam Coverage

✅ **Cluster Architecture, Installation & Configuration** (15%)
✅ **Workloads & Scheduling** (15%)
✅ **Services & Networking** (20%)
✅ **Storage** (10%)
✅ **Troubleshooting** (30%)
✅ **Security** (10%)

## How to Use

1. **Start with Basics**: Begin with 01-core-concepts
2. **Progress Systematically**: Work through each folder in order
3. **Practice Regularly**: Aim for 2-3 scenarios per day
4. **Time Yourself**: Practice under exam conditions (2 hours)
5. **Review Weak Areas**: Focus extra time on troubleshooting (30% of exam)
6. **Use Quick Reference**: Keep QUICK_REFERENCE.md handy
7. **Take Practice Exams**: Complete all 8 practice exam scenarios

## Success Tips

- **Practice Daily**: Consistency is key
- **Understand, Don't Memorize**: Know why each solution works
- **Time Management**: ~6-8 minutes per question
- **Use kubectl explain**: Your best friend during practice
- **Verify Everything**: Always check your work
- **Clean Up**: Delete resources after practice to avoid confusion

Good luck with your CKA exam preparation! 🚀

