# CKA Exam Quick Reference

> **Note**: For a comprehensive command cheat sheet, see `KUBERNETES_CHEAT_SHEET.md`

## Essential kubectl Commands

### Resource Management
```bash
# Create resources
kubectl create deployment <name> --image=<image>
kubectl create service clusterip <name> --tcp=80:80
kubectl run <pod-name> --image=<image> --restart=Never

# Apply YAML
kubectl apply -f <file.yaml>
kubectl create -f <file.yaml>

# Get resources
kubectl get <resource> [<name>]
kubectl get pods,svc,deploy -A
kubectl get all -n <namespace>

# Describe resources
kubectl describe <resource> <name>
kubectl describe pod <pod-name>

# Delete resources
kubectl delete <resource> <name>
kubectl delete -f <file.yaml>
kubectl delete pods --all
```

### Output Formats
```bash
kubectl get pod <name> -o yaml
kubectl get pod <name> -o json
kubectl get pod <name> -o jsonpath='{.spec.containers[0].image}'
kubectl get pod <name> -o wide
kubectl get pod <name> -o custom-columns=NAME:.metadata.name,IMAGE:.spec.containers[0].image
```

### Editing Resources
```bash
kubectl edit <resource> <name>
kubectl patch deployment <name> -p '{"spec":{"replicas":5}}'
kubectl set image deployment/<name> <container>=<new-image>
kubectl scale deployment <name> --replicas=3
```

### Debugging
```bash
# Logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl logs <pod-name> -c <container-name>
kubectl logs -f <pod-name>

# Execute commands
kubectl exec -it <pod-name> -- /bin/sh
kubectl exec <pod-name> -- <command>

# Port forwarding
kubectl port-forward <pod-name> 8080:80
kubectl port-forward svc/<service-name> 8080:80

# Debug pod
kubectl debug <pod-name> -it --image=busybox:1.35
```

### Events and Troubleshooting
```bash
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get events -n <namespace>
kubectl top nodes
kubectl top pods
kubectl top pods --containers
```

### Rollouts
```bash
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>
kubectl rollout undo deployment/<name> --to-revision=2
kubectl rollout pause deployment/<name>
kubectl rollout resume deployment/<name>
```

### Node Management
```bash
kubectl get nodes
kubectl describe node <node-name>
kubectl cordon <node-name>
kubectl uncordon <node-name>
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data
kubectl label nodes <node-name> <key>=<value>
kubectl taint nodes <node-name> <key>=<value>:<effect>
```

### RBAC
```bash
kubectl get role,rolebinding
kubectl get clusterrole,clusterrolebinding
kubectl auth can-i create pods
kubectl auth can-i --list --as=system:serviceaccount:default:my-sa
```

### ConfigMaps and Secrets
```bash
kubectl create configmap <name> --from-literal=<key>=<value>
kubectl create configmap <name> --from-file=<file>
kubectl create secret generic <name> --from-literal=<key>=<value>
kubectl create secret tls <name> --cert=<cert> --key=<key>
kubectl get secret <name> -o jsonpath='{.data.<key>}' | base64 -d
```

### Networking
```bash
kubectl get svc,endpoints
kubectl get ingress
kubectl get networkpolicy
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- nslookup <service-name>
```

### Storage
```bash
kubectl get pv,pvc
kubectl get storageclass
kubectl describe pvc <pvc-name>
```

### Namespaces
```bash
kubectl get namespaces
kubectl create namespace <name>
kubectl get all -n <namespace>
kubectl config set-context --current --namespace=<namespace>
```

## Common YAML Templates

### Pod
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

### Deployment
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

### Service
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

### ConfigMap
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: <name>
data:
  key: value
```

### Secret
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: <name>
type: Opaque
stringData:
  key: value
```

## Exam Tips

1. **Time Management**: 2 hours for 15-20 questions (~6-8 min per question)
2. **Use kubectl explain**: `kubectl explain pod.spec.containers`
3. **Generate YAML**: `kubectl create deployment <name> --image=<img> --dry-run=client -o yaml`
4. **Copy-paste**: Use `kubectl get <resource> <name> -o yaml` to get existing YAML
5. **Aliases**: Set up `alias k=kubectl` to save time
6. **Context switching**: `kubectl config use-context <context-name>`
7. **Namespace**: Always check current namespace: `kubectl config view --minify`

## Common Issues and Solutions

### Pod Pending
- Check resources: `kubectl describe pod <name>`
- Check node capacity: `kubectl describe node`
- Check PVC binding: `kubectl get pvc`

### Pod CrashLoopBackOff
- Check logs: `kubectl logs <pod-name> --previous`
- Check events: `kubectl get events`

### Service No Endpoints
- Check selector: `kubectl get svc <name> -o yaml`
- Check pod labels: `kubectl get pods --show-labels`

### ImagePullBackOff
- Check image name and tag
- Check image pull secrets if private registry

