# Scenario 5: Troubleshooting

## Scenario 5.1: Pod Issues

### Question 5.1.1: Pod in Pending State
A pod named `pending-pod` is stuck in Pending state. Diagnose and fix.

**Diagnosis Steps:**
1. Check pod status: `kubectl get pod pending-pod`
2. Describe pod: `kubectl describe pod pending-pod`
3. Check events: `kubectl get events --sort-by=.metadata.creationTimestamp`
4. Common causes:
   - Insufficient resources
   - Node selector/affinity issues
   - PVC not bound
   - Taints/tolerations mismatch

**Solution:**
- Check `solutions/pod-pending-issue.yaml` for example problematic pod
- Fix by adjusting resources, node selectors, or PVC

### Question 5.1.2: Pod in CrashLoopBackOff
A pod named `crashing-pod` is in CrashLoopBackOff. Diagnose and fix.

**Diagnosis Steps:**
1. Check pod status: `kubectl get pod crashing-pod`
2. Check logs: `kubectl logs crashing-pod`
3. Check previous logs: `kubectl logs crashing-pod --previous`
4. Describe pod: `kubectl describe pod crashing-pod`
5. Common causes:
   - Application errors
   - Missing environment variables
   - Wrong image or command
   - Resource limits too low

**Solution:**
- Check `solutions/pod-crashloop-issue.yaml` for example
- Fix by correcting image, command, or environment

### Question 5.1.3: Pod Not Ready
A pod is running but not ready. Diagnose and fix.

**Diagnosis Steps:**
1. Check readiness probe: `kubectl describe pod <pod-name> | grep -A 5 Readiness`
2. Check if probe endpoint is accessible
3. Test manually: `kubectl exec <pod-name> -- curl localhost:<port>/health`

**Solution:**
- Fix readiness probe configuration
- Ensure health endpoint exists

## Scenario 5.2: Service Issues

### Question 5.2.1: Service Not Routing Traffic
A service `web-service` exists but traffic is not reaching pods.

**Diagnosis Steps:**
1. Check service: `kubectl get svc web-service`
2. Check endpoints: `kubectl get endpoints web-service`
3. Verify selector matches pod labels: `kubectl get pods --show-labels`
4. Check service selector: `kubectl get svc web-service -o yaml | grep selector`

**Solution:**
- Ensure service selector matches pod labels
- Check `solutions/service-no-endpoints.yaml` for example issue

### Question 5.2.2: Service Endpoints Empty
Service has no endpoints. Diagnose and fix.

**Diagnosis Steps:**
1. `kubectl get endpoints <service-name>`
2. Check if pods exist with matching labels
3. Verify pod is running (not pending/crashloop)

**Solution:**
- Fix pod labels or service selector
- Ensure pods are running

## Scenario 5.3: Deployment Issues

### Question 5.3.1: Deployment Not Updating
Deployment update is not rolling out. Diagnose and fix.

**Diagnosis Steps:**
1. Check rollout status: `kubectl rollout status deployment <name>`
2. Check rollout history: `kubectl rollout history deployment <name>`
3. Describe deployment: `kubectl describe deployment <name>`
4. Check ReplicaSet: `kubectl get rs -l app=<app-label>`

**Solution:**
- Check `solutions/deployment-rollout-issue.yaml`
- Fix image pull issues, resource constraints, or probe failures

### Question 5.3.2: Deployment Rollback
Rollback a deployment to previous revision.

**Solution Steps:**
1. Check history: `kubectl rollout history deployment <name>`
2. Rollback: `kubectl rollout undo deployment <name>`
3. Rollback to specific revision: `kubectl rollout undo deployment <name> --to-revision=2`
4. Verify: `kubectl rollout status deployment <name>`

## Scenario 5.4: Node Issues

### Question 5.4.1: Node Not Ready
A node is in NotReady state. Diagnose.

**Diagnosis Steps:**
1. Check nodes: `kubectl get nodes`
2. Describe node: `kubectl describe node <node-name>`
3. Check kubelet status: `systemctl status kubelet` (on node)
4. Check node conditions: `kubectl get node <node-name> -o yaml | grep -A 5 conditions`

**Common Issues:**
- Kubelet not running
- Network issues
- Disk pressure
- Memory pressure

## Scenario 5.5: Network Issues

### Question 5.5.1: Pod Cannot Reach Service
Pod cannot connect to a service. Diagnose.

**Diagnosis Steps:**
1. Test DNS: `kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- nslookup <service-name>`
2. Test connectivity: `kubectl exec <pod> -- wget -O- http://<service-name>`
3. Check NetworkPolicies: `kubectl get networkpolicy`
4. Check service endpoints

**Solution:**
- Fix NetworkPolicy if blocking traffic
- Verify service and endpoints exist

## Scenario 5.6: Storage Issues

### Question 5.6.1: PVC Not Bound
PVC is in Pending state. Diagnose and fix.

**Diagnosis Steps:**
1. Check PVC: `kubectl get pvc`
2. Describe PVC: `kubectl describe pvc <pvc-name>`
3. Check PVs: `kubectl get pv`
4. Check StorageClass: `kubectl get storageclass`

**Common Issues:**
- No available PV matching requirements
- StorageClass not found
- Access mode mismatch

**Solution:**
- Create matching PV
- Fix StorageClass configuration

### Question 5.1.4: Pod ImagePullBackOff
A pod is in ImagePullBackOff state. Diagnose and fix.

**Diagnosis Steps:**
1. Check pod status: `kubectl get pod <pod-name>`
2. Describe pod: `kubectl describe pod <pod-name>`
3. Check events for image pull errors
4. Common causes:
   - Image doesn't exist
   - Wrong image tag
   - Private registry without credentials
   - Network issues

**Solution:**
- Fix image name/tag
- Add image pull secrets if private registry
- Verify image exists

### Question 5.1.5: Pod OOMKilled
A pod is being killed due to OOM (Out of Memory). Diagnose and fix.

**Diagnosis Steps:**
1. Check pod status: `kubectl get pod <pod-name>`
2. Check events: `kubectl get events | grep <pod-name>`
3. Check resource usage: `kubectl top pod <pod-name>`
4. Check memory limits: `kubectl describe pod <pod-name> | grep -A 5 Limits`

**Solution:**
- Increase memory limit
- Optimize application memory usage
- Check for memory leaks

### Question 5.1.6: Pod Startup Probe Failure
A pod fails startup probe. Diagnose and fix.

**Diagnosis Steps:**
1. Check startup probe config: `kubectl describe pod <pod-name> | grep -A 5 Startup`
2. Check application logs
3. Verify startup probe endpoint exists
4. Check startup probe timing

**Solution:**
- Adjust startup probe timing
- Fix application startup
- Ensure probe endpoint is accessible

### Question 5.1.7: Pod with Init Container Failure
A pod's init container is failing. Diagnose and fix.

**Diagnosis Steps:**
1. Check pod status: `kubectl get pod <pod-name>`
2. Check init container logs: `kubectl logs <pod-name> -c <init-container-name>`
3. Describe pod for init container status

**Solution:**
- Fix init container command/script
- Ensure init container dependencies exist
- Check init container resource limits

## Scenario 5.2: Service Issues

### Question 5.2.3: Service Port Mismatch
Service exists but connection fails. Diagnose port mismatch.

**Diagnosis Steps:**
1. Check service port: `kubectl get svc <service-name> -o yaml | grep -A 5 ports`
2. Check pod port: `kubectl get pod <pod-name> -o yaml | grep containerPort`
3. Test connectivity: `kubectl exec <pod> -- curl http://<service-name>:<port>`

**Solution:**
- Fix service targetPort to match pod containerPort
- Verify port mapping

### Question 5.2.4: Service DNS Not Resolving
Service DNS name not resolving. Diagnose.

**Diagnosis Steps:**
1. Test DNS: `kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- nslookup <service-name>`
2. Check service exists: `kubectl get svc <service-name>`
3. Check namespace: Ensure correct namespace
4. Test FQDN: `<service-name>.<namespace>.svc.cluster.local`

**Solution:**
- Verify service exists
- Use correct namespace in DNS query
- Check CoreDNS is running

### Question 5.2.5: Service Session Affinity Not Working
Service with session affinity not maintaining sessions. Diagnose.

**Diagnosis Steps:**
1. Check service config: `kubectl get svc <service-name> -o yaml | grep sessionAffinity`
2. Verify sessionAffinity is set to ClientIP
3. Check timeout: `kubectl get svc <service-name> -o yaml | grep timeoutSeconds`

**Solution:**
- Ensure sessionAffinity is ClientIP
- Adjust timeoutSeconds if needed
- Verify client IP is preserved

## Scenario 5.3: Deployment Issues

### Question 5.3.3: Deployment Replica Mismatch
Deployment shows desired replicas but actual replicas don't match.

**Diagnosis Steps:**
1. Check deployment: `kubectl get deployment <name>`
2. Check ReplicaSet: `kubectl get rs -l app=<label>`
3. Check pods: `kubectl get pods -l app=<label>`
4. Describe deployment: `kubectl describe deployment <name>`

**Solution:**
- Check resource quotas
- Check node capacity
- Check pod disruption budgets
- Fix resource constraints

### Question 5.3.4: Deployment Rolling Update Stuck
Deployment rolling update is stuck. Diagnose and fix.

**Diagnosis Steps:**
1. Check rollout status: `kubectl rollout status deployment <name>`
2. Check events: `kubectl get events | grep <deployment-name>`
3. Check new ReplicaSet: `kubectl get rs`
4. Check pod status in new RS

**Solution:**
- Fix image pull issues
- Fix resource constraints
- Fix probe failures
- Check maxSurge/maxUnavailable settings

### Question 5.3.5: Deployment History Issues
Deployment history is missing or incorrect.

**Diagnosis Steps:**
1. Check history: `kubectl rollout history deployment <name>`
2. Check revision annotation: `kubectl get deployment <name> -o yaml | grep revisionHistoryLimit`

**Solution:**
- Set revisionHistoryLimit appropriately
- Understand history is limited

## Scenario 5.4: Node Issues

### Question 5.4.2: Node Disk Pressure
Node shows disk pressure. Diagnose and fix.

**Diagnosis Steps:**
1. Check node conditions: `kubectl describe node <node-name> | grep -A 5 Conditions`
2. Check disk usage on node
3. Check events: `kubectl get events | grep <node-name>`

**Solution:**
- Clean up unused images: `docker system prune` (if using Docker)
- Clean up unused volumes
- Add more disk space
- Evict pods if needed

### Question 5.4.3: Node Memory Pressure
Node shows memory pressure. Diagnose.

**Diagnosis Steps:**
1. Check node conditions
2. Check node resources: `kubectl describe node <node-name> | grep -A 10 "Allocated resources"`
3. Check pod resource usage: `kubectl top pods --all-namespaces`

**Solution:**
- Identify memory-hungry pods
- Adjust pod resource requests/limits
- Add more nodes
- Evict low-priority pods

### Question 5.4.4: Node Network Issues
Node network connectivity issues. Diagnose.

**Diagnosis Steps:**
1. Check node status: `kubectl get nodes`
2. Check node conditions
3. Test network from node
4. Check kubelet status

**Solution:**
- Fix network configuration
- Restart kubelet
- Check CNI plugin status

### Question 5.4.5: Node Not Ready - Kubelet Issues
Node is NotReady due to kubelet problems.

**Diagnosis Steps:**
1. Check node: `kubectl describe node <node-name>`
2. Check kubelet logs on node
3. Check kubelet service status
4. Verify certificates

**Solution:**
- Restart kubelet: `systemctl restart kubelet`
- Check kubelet configuration
- Verify certificates are valid
- Check disk space for kubelet

## Scenario 5.5: Network Issues

### Question 5.5.2: NetworkPolicy Blocking Traffic
NetworkPolicy is blocking legitimate traffic. Diagnose.

**Diagnosis Steps:**
1. List NetworkPolicies: `kubectl get networkpolicy`
2. Describe policies: `kubectl describe networkpolicy <policy-name>`
3. Check pod labels match policy selectors
4. Test connectivity with and without policy

**Solution:**
- Adjust NetworkPolicy rules
- Ensure pod labels match policy selectors
- Add missing ingress/egress rules

### Question 5.5.3: Ingress Not Routing
Ingress exists but not routing traffic. Diagnose.

**Diagnosis Steps:**
1. Check ingress: `kubectl get ingress <ingress-name>`
2. Check ingress controller: `kubectl get pods -n ingress-nginx` (or appropriate namespace)
3. Check ingress status: `kubectl describe ingress <ingress-name>`
4. Test ingress endpoint

**Solution:**
- Verify ingress controller is running
- Check ingress rules are correct
- Verify backend services exist
- Check ingress class annotation

## Scenario 5.6: Storage Issues

### Question 5.6.2: PVC Not Binding
PVC is Pending and not binding to PV. Diagnose.

**Diagnosis Steps:**
1. Check PVC: `kubectl describe pvc <pvc-name>`
2. Check available PVs: `kubectl get pv`
3. Compare PVC requirements with PV specs
4. Check StorageClass: `kubectl get storageclass`

**Solution:**
- Create matching PV
- Fix StorageClass configuration
- Adjust PVC requirements
- Check access mode compatibility

### Question 5.6.3: Pod Cannot Mount Volume
Pod cannot mount PVC. Diagnose.

**Diagnosis Steps:**
1. Check pod status: `kubectl describe pod <pod-name>`
2. Check events: `kubectl get events | grep <pod-name>`
3. Verify PVC is bound: `kubectl get pvc <pvc-name>`
4. Check volume mount path

**Solution:**
- Ensure PVC is bound
- Fix volume mount configuration
- Check node has access to storage
- Verify StorageClass provisioner is working

### Question 5.6.4: Volume Mount Read-Only
Volume is mounted as read-only unexpectedly.

**Diagnosis Steps:**
1. Check pod volume mounts: `kubectl describe pod <pod-name> | grep -A 10 Mounts`
2. Check PVC access mode
3. Check security context

**Solution:**
- Ensure PVC access mode allows writing
- Check security context readOnlyRootFilesystem
- Verify volume mount readOnly setting

## Scenario 5.7: RBAC Issues

### Question 5.7.1: Permission Denied
ServiceAccount cannot perform action. Diagnose RBAC.

**Diagnosis Steps:**
1. Check permissions: `kubectl auth can-i <verb> <resource> --as=system:serviceaccount:<ns>:<sa>`
2. List all permissions: `kubectl auth can-i --list --as=system:serviceaccount:<ns>:<sa>`
3. Check RoleBindings: `kubectl get rolebinding,clusterrolebinding`
4. Check Roles: `kubectl describe role <role-name>`

**Solution:**
- Create/update Role with required permissions
- Create/update RoleBinding
- Verify ServiceAccount is correct

### Question 5.7.2: ServiceAccount Token Missing
Pod cannot access API server. Check ServiceAccount.

**Diagnosis Steps:**
1. Check pod ServiceAccount: `kubectl get pod <pod-name> -o jsonpath='{.spec.serviceAccountName}'`
2. Check token mount: `kubectl exec <pod-name> -- ls /var/run/secrets/kubernetes.io/serviceaccount`
3. Check ServiceAccount exists

**Solution:**
- Ensure ServiceAccount exists
- Verify pod uses correct ServiceAccount
- Check automountServiceAccountToken is not false

## Scenario 5.8: Resource Quota Issues

### Question 5.8.1: Cannot Create Pod - Quota Exceeded
Cannot create pod due to resource quota.

**Diagnosis Steps:**
1. Check ResourceQuota: `kubectl get resourcequota -n <namespace>`
2. Check quota usage: `kubectl describe resourcequota <quota-name> -n <namespace>`
3. Check pod resources: `kubectl describe pod <pod-name> | grep -A 5 "Limits\|Requests"`

**Solution:**
- Reduce pod resource requests
- Increase ResourceQuota limits
- Delete unused resources
- Move to different namespace

## Practice Commands

```bash
# Pod troubleshooting
kubectl get pods
kubectl get pods -A
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
kubectl logs <pod-name> -c <container-name>
kubectl logs <pod-name> --all-containers=true
kubectl exec <pod-name> -- <command>
kubectl exec -it <pod-name> -- /bin/sh
kubectl top pod <pod-name>
kubectl top pods --containers

# Service troubleshooting
kubectl get svc
kubectl get svc -A
kubectl get endpoints
kubectl describe svc <service-name>
kubectl get svc <service-name> -o yaml
kubectl port-forward svc/<service-name> 8080:80

# Deployment troubleshooting
kubectl get deployment
kubectl describe deployment <name>
kubectl rollout status deployment <name>
kubectl rollout history deployment <name>
kubectl rollout undo deployment <name>
kubectl rollout undo deployment <name> --to-revision=2
kubectl get rs -l app=<label>
kubectl describe rs <rs-name>

# Node troubleshooting
kubectl get nodes
kubectl get nodes -o wide
kubectl describe node <node-name>
kubectl top nodes
kubectl cordon <node-name>
kubectl uncordon <node-name>
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data

# Events
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get events -n <namespace>
kubectl get events --field-selector involvedObject.name=<pod-name>

# Resource usage
kubectl top nodes
kubectl top pods
kubectl top pods --all-namespaces
kubectl top pods --containers

# Debugging
kubectl run debug --image=busybox:1.35 --rm -it --restart=Never -- sh
kubectl debug <pod-name> -it --image=busybox:1.35
kubectl get pod <pod-name> -o yaml > pod-debug.yaml

# Network troubleshooting
kubectl get networkpolicy
kubectl describe networkpolicy <policy-name>
kubectl get ingress
kubectl describe ingress <ingress-name>

# Storage troubleshooting
kubectl get pv,pvc
kubectl describe pvc <pvc-name>
kubectl describe pv <pv-name>
kubectl get storageclass

# RBAC troubleshooting
kubectl auth can-i <verb> <resource>
kubectl auth can-i <verb> <resource> --as=system:serviceaccount:<ns>:<sa>
kubectl auth can-i --list
kubectl auth can-i --list --as=system:serviceaccount:<ns>:<sa>
kubectl get role,rolebinding
kubectl describe role <role-name>
kubectl describe rolebinding <binding-name>
```

