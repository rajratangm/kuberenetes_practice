# Kubernetes One-Page Cheat Sheet - CKA Exam

## Most Used Commands

### Get & Describe
```bash
kubectl get <resource> <name>                    # Get resource
kubectl get pods -o wide                        # Wide output
kubectl get all -n <ns>                         # All resources
kubectl describe <resource> <name>              # Detailed info
kubectl get events --sort-by=.lastTimestamp      # Recent events
```

### Pods
```bash
kubectl logs <pod>                              # Logs
kubectl logs <pod> --previous                   # Previous container
kubectl exec -it <pod> -- /bin/sh               # Execute
kubectl port-forward <pod> 8080:80              # Port forward
```

### Deployments
```bash
kubectl set image deploy/<name> <c>=<img>       # Update image
kubectl scale deploy <name> --replicas=5        # Scale
kubectl rollout status deploy/<name>            # Status
kubectl rollout undo deploy/<name>              # Rollback
kubectl rollout history deploy/<name>           # History
```

### Services
```bash
kubectl get svc,ep                              # Service & endpoints
kubectl expose deploy <name> --port=80          # Expose
kubectl port-forward svc/<name> 8080:80         # Port forward
```

### Create Resources
```bash
kubectl run <name> --image=<img>                # Pod
kubectl create deploy <name> --image=<img>      # Deployment
kubectl create cm <name> --from-literal=k=v     # ConfigMap
kubectl create secret generic <name> --from-literal=k=v  # Secret
kubectl create ns <name>                        # Namespace
```

### RBAC
```bash
kubectl create role <name> --verb=get,list --resource=pods
kubectl create rolebinding <name> --role=<role> --serviceaccount=<ns>:<sa>
kubectl auth can-i create pods --as=system:serviceaccount:<ns>:<sa>
```

### Storage
```bash
kubectl get pv,pvc                              # Volumes
kubectl get sc                                   # StorageClass
```

### Node Management
```bash
kubectl cordon <node>                           # Cordon
kubectl uncordon <node>                         # Uncordon
kubectl drain <node> --ignore-daemonsets        # Drain
kubectl label nodes <node> <key>=<value>        # Label
kubectl taint nodes <node> <key>=<value>:<effect>  # Taint
```

### Troubleshooting
```bash
kubectl get pods                                # Check status
kubectl describe pod <name>                     # Details
kubectl logs <pod>                              # Logs
kubectl get events                              # Events
kubectl top pods                                # Resource usage
kubectl get endpoints <svc>                     # Service endpoints
```

### Output Formats
```bash
kubectl get <resource> -o yaml                  # YAML
kubectl get <resource> -o json                  # JSON
kubectl get <resource> -o wide                  # Wide
kubectl explain <resource>.<field>             # Explain
```

### Generate YAML
```bash
kubectl run <name> --image=<img> --dry-run=client -o yaml
kubectl create deploy <name> --image=<img> --dry-run=client -o yaml
```

### Short Names
```bash
po,svc,deploy,rs,cm,secret,ns,pv,pvc,sc,ing,netpol,sa,sts,ds,cj,pdb
```

### Quick Fixes
```bash
# Service no endpoints
kubectl get svc <name> -o yaml | grep selector
kubectl get pods --show-labels

# Pod pending
kubectl describe pod <name>
kubectl get events --field-selector involvedObject.name=<pod>

# Deployment stuck
kubectl rollout status deploy/<name>
kubectl get rs
kubectl describe pod <new-pod>

# PVC not binding
kubectl describe pvc <name>
kubectl get pv
kubectl get sc
```

### Time Savers
```bash
alias k=kubectl                                 # Alias
kubectl get pods,svc,deploy                     # Multiple resources
kubectl get pods -l app=web                     # Label selector
kubectl get pods -w                             # Watch mode
kubectl get all -A                              # All namespaces
```

---

**Print this page for quick reference during practice!**

