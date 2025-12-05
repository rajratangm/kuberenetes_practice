# CKA Exam Day Checklist

## Pre-Exam Preparation

### Technical Setup
- [ ] Kubernetes cluster access verified
- [ ] kubectl configured and working
- [ ] Can access Kubernetes documentation (allowed during exam)
- [ ] Browser bookmarks for kubectl cheat sheet
- [ ] Terminal/command line ready

### Knowledge Check
- [ ] Can create basic resources (Pod, Deployment, Service) in < 3 minutes
- [ ] Can troubleshoot common issues in < 5 minutes
- [ ] Familiar with kubectl explain command
- [ ] Know how to use --dry-run=client -o yaml
- [ ] Understand all CKA exam domains

### Practice Metrics
- [ ] Can complete basic scenarios in < 5 minutes
- [ ] Can complete intermediate scenarios in < 8 minutes
- [ ] Can complete advanced scenarios in < 12 minutes
- [ ] Can complete full 2-hour exam simulation
- [ ] Accuracy rate > 90% on practice scenarios

## Exam Day Strategy

### Time Management (2 hours total)
- **Troubleshooting (30%)**: ~36 minutes
- **Workloads & Scheduling (15%)**: ~18 minutes
- **Services & Networking (20%)**: ~24 minutes
- **Storage (10%)**: ~12 minutes
- **Security (10%)**: ~12 minutes
- **Cluster Maintenance (15%)**: ~18 minutes

### Question Approach
1. **Read Carefully**: Understand all requirements
2. **Plan First**: Think about approach before executing
3. **Verify**: Always verify your solution works
4. **Move On**: Don't spend > 10 minutes on one question
5. **Review**: Leave 10 minutes at end for review

## Common Tasks - Time Targets

### 1-Minute Tasks
- Get pod logs
- Describe resource
- Check service endpoints
- Get events
- Check node status
- Scale deployment
- Get resource YAML

### 2-Minute Tasks
- Create simple pod
- Create ConfigMap/Secret
- Check resource usage
- Cordon/uncordon node
- Get rollout status

### 3-Minute Tasks
- Update deployment image
- Create Service
- Rollback deployment
- Create Role/RoleBinding
- Fix simple pod issue

### 5-Minute Tasks
- Create Deployment
- Configure NetworkPolicy
- Create PVC and use in pod
- Set up basic RBAC
- Fix service connectivity

### 10-Minute Tasks
- Complete application setup
- Troubleshoot multiple issues
- Configure Ingress
- Set up StatefulSet
- Implement security policies

## Quick Reference Commands

### Diagnostic
```bash
kubectl get <resource> <name>
kubectl describe <resource> <name>
kubectl get events --sort-by=.lastTimestamp
kubectl logs <pod-name>
kubectl logs <pod-name> --previous
```

### Creation (Imperative - Faster)
```bash
kubectl run <name> --image=<image>
kubectl create deployment <name> --image=<image> --replicas=3
kubectl create service clusterip <name> --tcp=80:80
kubectl create configmap <name> --from-literal=key=value
kubectl create secret generic <name> --from-literal=key=value
```

### Updates
```bash
kubectl set image deployment/<name> <container>=<new-image>
kubectl scale deployment <name> --replicas=5
kubectl rollout undo deployment/<name>
kubectl rollout status deployment/<name>
```

### Generate YAML
```bash
kubectl create <resource> <name> --image=<image> --dry-run=client -o yaml
```

## Troubleshooting Checklist

### Pod Issues
- [ ] Check pod status: `kubectl get pod <name>`
- [ ] Describe pod: `kubectl describe pod <name>`
- [ ] Check logs: `kubectl logs <pod-name>`
- [ ] Check events: `kubectl get events`
- [ ] Verify resources available
- [ ] Check node capacity
- [ ] Verify labels/selectors

### Service Issues
- [ ] Check service: `kubectl get svc <name>`
- [ ] Check endpoints: `kubectl get endpoints <name>`
- [ ] Verify selector matches pod labels
- [ ] Test connectivity
- [ ] Check NetworkPolicy

### Deployment Issues
- [ ] Check deployment status
- [ ] Check rollout status
- [ ] Check ReplicaSet
- [ ] Check pod status
- [ ] Check events
- [ ] Verify image pull

### Storage Issues
- [ ] Check PVC status
- [ ] Check PV availability
- [ ] Verify StorageClass
- [ ] Check access modes
- [ ] Verify binding

### RBAC Issues
- [ ] Check ServiceAccount
- [ ] Check Role/RoleBinding
- [ ] Test permissions: `kubectl auth can-i`
- [ ] Verify namespace
- [ ] Check resource/verb combinations

## Common Mistakes to Avoid

- [ ] Not checking current namespace
- [ ] Label mismatches (case-sensitive)
- [ ] Port mismatches
- [ ] Forgetting to verify solution
- [ ] Spending too long on one question
- [ ] Not using kubectl explain when stuck
- [ ] Not checking events for errors
- [ ] Wrong resource type (Deployment vs StatefulSet)
- [ ] Forgetting resource limits
- [ ] Not testing connectivity

## Verification Checklist

After completing each task:
- [ ] Resource created/updated successfully
- [ ] Status shows expected state (Running, Bound, etc.)
- [ ] Can access/use the resource
- [ ] No errors in events
- [ ] Related resources work together
- [ ] Meets all requirements

## Emergency Procedures

### If Stuck on a Question
1. Check events: `kubectl get events`
2. Use kubectl explain: `kubectl explain <resource>.<field>`
3. Check documentation (allowed during exam)
4. Try different approach
5. If > 10 minutes, move on and come back

### If Cluster Issues
1. Check node status: `kubectl get nodes`
2. Check system pods: `kubectl get pods -n kube-system`
3. Report to proctor if persistent
4. Continue with other questions

### If Time Running Out
1. Prioritize high-value questions
2. Do quick fixes first
3. Skip complex scenarios
4. Verify critical resources work
5. Document what you did

## Post-Exam

- [ ] Review all answers if time permits
- [ ] Verify critical resources are working
- [ ] Check for any obvious mistakes
- [ ] Ensure all requirements met
- [ ] Submit with confidence

## Final Tips

1. **Stay Calm**: You've practiced, trust your knowledge
2. **Read Carefully**: Misreading requirements wastes time
3. **Verify Everything**: Don't assume it works, test it
4. **Use Explain**: `kubectl explain` is your friend
5. **Time Management**: Watch the clock, prioritize
6. **Documentation**: Use it when needed (it's allowed)
7. **Practice**: The more you practice, the faster you'll be

## Success Formula

**Preparation** + **Practice** + **Time Management** + **Verification** = **CKA Success**

You've got this! Good luck! 🚀

