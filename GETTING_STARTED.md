# Getting Started with CKA Practice

## Repository Structure

```
kubernetes_practice/
├── README.md                    # Main overview
├── QUICK_REFERENCE.md           # Essential kubectl commands
├── PRACTICE_EXAM.md             # Full practice exam scenarios
├── GETTING_STARTED.md           # This file
│
├── 01-core-concepts/            # Pods, Deployments, ReplicaSets
│   ├── README.md                # Questions and instructions
│   └── solutions/               # YAML solution files
│
├── 02-networking/               # Services, Ingress, NetworkPolicies
│   ├── README.md
│   └── solutions/
│
├── 03-storage/                  # PV, PVC, StorageClass
│   ├── README.md
│   └── solutions/
│
├── 04-security/                 # RBAC, Secrets, ServiceAccounts
│   ├── README.md
│   └── solutions/
│
├── 05-troubleshooting/          # Debugging scenarios
│   ├── README.md
│   └── solutions/
│
├── 06-cluster-maintenance/      # Upgrades, backups, quotas
│   ├── README.md
│   └── solutions/
│
├── 07-advanced-workloads/       # Jobs, CronJobs, DaemonSets, StatefulSets
│   ├── README.md
│   └── solutions/
│
└── 08-configuration/            # ConfigMaps, Affinity, PDB
    ├── README.md
    └── solutions/
```

## How to Use This Repository

### Step 1: Set Up Your Environment

1. **Install kubectl**: Ensure you have kubectl installed
   ```bash
   kubectl version --client
   ```

2. **Set Up a Kubernetes Cluster**:
   - **Option A**: Use minikube
     ```bash
     minikube start
     ```
   - **Option B**: Use kind (Kubernetes in Docker)
     ```bash
     kind create cluster
     ```
   - **Option C**: Use a cloud provider (GKE, EKS, AKS)

3. **Verify Cluster Access**:
   ```bash
   kubectl cluster-info
   kubectl get nodes
   ```

### Step 2: Practice by Topic

1. **Start with Core Concepts** (01-core-concepts):
   - Read the README.md in the folder
   - Try to solve questions yourself
   - Check solutions when stuck
   - Apply YAML files: `kubectl apply -f solutions/<file>.yaml`
   - Verify: `kubectl get pods,deployments,services`

2. **Move Through Each Topic**:
   - Follow the same pattern for each numbered folder
   - Practice commands from QUICK_REFERENCE.md
   - Test your understanding by modifying solutions

### Step 3: Practice Exams

1. **Use PRACTICE_EXAM.md**:
   - Complete each practice exam scenario
   - Time yourself (real exam is 2 hours)
   - Verify all requirements are met
   - Clean up resources after each exam

### Step 4: Review and Reinforce

1. **Use QUICK_REFERENCE.md**:
   - Memorize common commands
   - Practice generating YAML with `--dry-run=client -o yaml`
   - Learn to use `kubectl explain` effectively

## Learning Path

### Beginner Path (Week 1-2)
1. 01-core-concepts (Pods, Deployments)
2. 02-networking (Services)
3. 08-configuration (ConfigMaps, Secrets)

### Intermediate Path (Week 3-4)
1. 03-storage (PV, PVC)
2. 04-security (RBAC basics)
3. 07-advanced-workloads (Jobs, CronJobs)

### Advanced Path (Week 5-6)
1. 02-networking (Ingress, NetworkPolicies)
2. 04-security (Advanced RBAC)
3. 05-troubleshooting
4. 06-cluster-maintenance

### Exam Preparation (Week 7-8)
1. Complete all PRACTICE_EXAM.md scenarios
2. Review QUICK_REFERENCE.md daily
3. Practice time management
4. Focus on weak areas

## Common Workflows

### Creating a Resource
```bash
# Method 1: From YAML file
kubectl apply -f solutions/pod-example.yaml

# Method 2: Imperative (faster for exam)
kubectl create deployment web --image=nginx:1.21 --replicas=3

# Method 3: Generate YAML first, then apply
kubectl create deployment web --image=nginx:1.21 --dry-run=client -o yaml > web.yaml
kubectl apply -f web.yaml
```

### Verifying Resources
```bash
# Get resources
kubectl get pods,svc,deploy

# Describe for details
kubectl describe pod <pod-name>

# Check events
kubectl get events --sort-by=.metadata.creationTimestamp

# Check logs
kubectl logs <pod-name>
```

### Troubleshooting Workflow
1. Check resource status: `kubectl get <resource>`
2. Describe resource: `kubectl describe <resource> <name>`
3. Check logs: `kubectl logs <pod-name>`
4. Check events: `kubectl get events`
5. Verify configuration: `kubectl get <resource> <name> -o yaml`

## Tips for Success

1. **Practice Daily**: Even 30 minutes daily is better than long sessions
2. **Use Imperative Commands**: Faster for exam, but understand declarative too
3. **Time Yourself**: Practice under time pressure
4. **Read Documentation**: Use `kubectl explain` during practice
5. **Clean Up**: Delete resources after practice to avoid confusion
6. **Test Everything**: Don't just create resources, verify they work
7. **Understand Why**: Don't just copy YAML, understand what each field does

## Exam Day Tips

1. **Read Questions Carefully**: Understand all requirements
2. **Time Management**: ~6-8 minutes per question
3. **Use kubectl explain**: When unsure about YAML structure
4. **Verify Your Work**: Always check if resources are created correctly
5. **Context Switching**: Be aware of which context/namespace you're in
6. **Don't Panic**: If stuck, move on and come back

## Resources

- **Official Kubernetes Documentation**: https://kubernetes.io/docs/
- **kubectl Cheat Sheet**: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
- **CKA Exam Curriculum**: https://www.cncf.io/certification/cka/

## Next Steps

1. ✅ Set up your Kubernetes cluster
2. ✅ Start with 01-core-concepts
3. ✅ Work through each scenario folder
4. ✅ Complete practice exams
5. ✅ Review quick reference daily
6. ✅ Schedule your CKA exam!

Good luck with your CKA certification journey! 🚀

