# CKA Practice Exam 4 - Troubleshooting Focus

**Duration**: 2 hours  
**Total Tasks**: 18  
**Passing Score**: 66% (74% for CKA)

---

## Task 1: Pod ImagePullBackOff (8 minutes)

Pod `app-pod` is in `ImagePullBackOff` state. Diagnose and fix:
- Check the error
- Fix the image name or pull policy
- Verify pod starts

---

## Task 2: Pod OOMKilled (10 minutes)

Pod `memory-hungry` keeps getting `OOMKilled`. Fix by:
- Checking current resource limits
- Increasing memory limit appropriately
- Verifying pod runs successfully

---

## Task 3: Pod Pending - Resource Issue (10 minutes)

Pod `large-pod` is stuck in `Pending` state. Diagnose:
- Check resource requests vs node capacity
- Check node selector/affinity
- Fix and verify scheduling

---

## Task 4: Service No Endpoints (10 minutes)

Service `web-svc` has no endpoints. Fix:
- Check service selector
- Check pod labels
- Fix mismatch
- Verify endpoints created

---

## Task 5: Deployment Not Scaling (10 minutes)

Deployment `api-deploy` won't scale beyond 3 replicas. Check:
- ResourceQuota limits
- Node capacity
- Fix the issue
- Scale to 5 replicas

---

## Task 6: Deployment Rollout Stuck (12 minutes)

Deployment `frontend` rollout is stuck. Diagnose:
- Check rollout status
- Check ReplicaSet status
- Check pod status and events
- Fix readiness probe or image issue
- Complete rollout

---

## Task 7: PVC Not Binding (12 minutes)

PVC `data-pvc` is in `Pending` state. Check:
- StorageClass exists
- PV availability
- Access mode match
- Storage size match
- Fix and verify binding

---

## Task 8: StatefulSet Pod Not Starting (12 minutes)

StatefulSet `db-sts` pod 0 is not starting. Check:
- StorageClass availability
- PVC binding
- Node taints
- Resource limits
- Fix and verify

---

## Task 9: Job Not Completing (10 minutes)

Job `process-job` is not completing. Check:
- Job logs
- Active deadline
- Resource limits
- Command correctness
- Fix and verify completion

---

## Task 10: CronJob Not Running (10 minutes)

CronJob `backup-cron` is not creating jobs. Check:
- Suspend status
- Schedule syntax
- Controller manager
- Fix and verify job creation

---

## Task 11: NetworkPolicy Blocking Traffic (12 minutes)

Pods can't communicate after NetworkPolicy applied. Diagnose:
- Check NetworkPolicy rules
- Verify pod labels
- Test connectivity
- Fix policy rules
- Verify communication works

---

## Task 12: Ingress Not Routing (10 minutes)

Ingress `app-ingress` not routing traffic. Check:
- Ingress class
- Service exists
- Service ports match
- Host/path configuration
- Fix and verify routing

---

## Task 13: RBAC Permission Denied (12 minutes)

ServiceAccount `app-sa` can't list pods. Fix:
- Check Role/RoleBinding
- Verify ServiceAccount name
- Check namespace
- Fix permissions
- Verify access

---

## Task 14: Node NotReady (12 minutes)

Node `worker-2` is in `NotReady` state. Diagnose:
- Check node conditions
- Check kubelet status
- Check system resources
- Document fix steps (simulate)

---

## Task 15: Pod CrashLoopBackOff (10 minutes)

Pod `crash-pod` is in `CrashLoopBackOff`. Diagnose:
- Check logs
- Check previous container logs
- Check events
- Fix command or image
- Verify pod runs

---

## Task 16: ResourceQuota Exceeded (10 minutes)

Can't create new pods due to ResourceQuota. Fix:
- Check quota usage
- Delete unused resources
- Optimize resource requests
- Or increase quota
- Verify new pods can be created

---

## Task 17: DaemonSet Not on All Nodes (10 minutes)

DaemonSet `monitor-ds` missing on some nodes. Check:
- Node selector
- Tolerations
- Node labels
- Fix and verify all nodes have pod

---

## Task 18: ConfigMap Not Loading (10 minutes)

Pod can't access ConfigMap data. Check:
- ConfigMap exists
- Volume mount path
- ConfigMap name match
- Fix and verify access

---

## Scoring

Each task: ~5.5 points  
**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

**Good luck!** 🚀

