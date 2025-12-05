# Scenario 7: Advanced Workloads

## Scenario 7.1: Jobs

### Question 7.1.1: Create a Job
Create a Job that:
- Name: `data-processor`
- Image: `busybox:1.35`
- Command: `echo "Processing data" && sleep 30`
- Restart policy: Never
- Completions: 1

**Solution Steps:**
1. Check `solutions/job-data-processor.yaml`
2. Apply: `kubectl apply -f solutions/job-data-processor.yaml`
3. Watch: `kubectl get job data-processor -w`
4. Check logs: `kubectl logs job/data-processor`

### Question 7.1.2: Parallel Job
Create a Job that runs 3 completions with parallelism of 2:
- Name: `parallel-job`
- Image: `busybox:1.35`
- Command: `echo "Job $JOB_COMPLETION_INDEX" && sleep 10`

**Solution Steps:**
1. Check `solutions/job-parallel.yaml`
2. Apply and watch parallel execution
3. Verify: `kubectl get pods -l job-name=parallel-job`

### Question 7.1.3: Job with Backoff Limit
Create a Job with:
- Name: `retry-job`
- Backoff limit: 3
- Command that may fail initially

**Solution Steps:**
1. Check `solutions/job-retry.yaml`
2. Apply and observe retries

### Question 7.1.4: Job with Active Deadline
Create a Job with active deadline:
- Name: `deadline-job`
- Active deadline: 300 seconds (5 minutes)
- Job will be terminated if not completed in time

**Solution Steps:**
1. Check `solutions/job-deadline.yaml`
2. Apply and verify deadline behavior

### Question 7.1.5: Job with TTL
Create a Job with TTL (Time To Live):
- Name: `ttl-job`
- TTL: 100 seconds
- Job and pods will be cleaned up after TTL expires

**Solution Steps:**
1. Check `solutions/job-ttl.yaml`
2. Apply and observe cleanup after completion

### Question 7.1.6: Job with Completion Mode
Create a Job with indexed completion mode:
- Name: `indexed-job`
- Completions: 5
- Completion mode: Indexed
- Each pod gets completion index

**Solution Steps:**
1. Check `solutions/job-indexed.yaml`
2. Apply and verify indexed completion
3. Check: `kubectl get pods -l job-name=indexed-job`

## Scenario 7.2: CronJobs

### Question 7.2.1: Basic CronJob
Create a CronJob that:
- Name: `backup-job`
- Schedule: `0 2 * * *` (runs at 2 AM daily)
- Image: `busybox:1.35`
- Command: `echo "Backup completed" && date`

**Solution Steps:**
1. Check `solutions/cronjob-backup.yaml`
2. Apply: `kubectl apply -f solutions/cronjob-backup.yaml`
3. Verify: `kubectl get cronjob backup-job`
4. Check jobs created: `kubectl get jobs -l app=backup`
5. Manually trigger: `kubectl create job --from=cronjob/backup-job backup-manual-$(date +%s)`

### Question 7.2.2: CronJob with Concurrency Policy
Create a CronJob with:
- Name: `cleanup-job`
- Schedule: `*/5 * * * *` (every 5 minutes)
- Concurrency policy: Forbid (prevents overlapping runs)
- Successful jobs history: 3
- Failed jobs history: 1

**Solution Steps:**
1. Check `solutions/cronjob-cleanup.yaml`
2. Apply and verify concurrency policy

### Question 7.2.3: CronJob with Suspend
Create a CronJob and suspend it:
- Name: `suspended-job`
- Schedule: `0 * * * *` (every hour)
- Suspend: true
- Verify it doesn't create jobs

**Solution Steps:**
1. Create CronJob with suspend: true
2. Verify: `kubectl get cronjob suspended-job`
3. Check no jobs created
4. Unsuspend: `kubectl patch cronjob suspended-job -p '{"spec":{"suspend":false}}'`

### Question 7.2.4: CronJob with Starting Deadline
Create a CronJob with starting deadline:
- Name: `deadline-cronjob`
- Schedule: `*/10 * * * *`
- Starting deadline: 200 seconds
- Job must start within deadline

**Solution Steps:**
1. Check `solutions/cronjob-deadline.yaml`
2. Apply and verify deadline behavior

### Question 7.2.5: CronJob Schedule Expressions
Understand and create CronJobs with different schedules:
- Every minute: `* * * * *`
- Every hour: `0 * * * *`
- Every day at 2 AM: `0 2 * * *`
- Every Monday at 9 AM: `0 9 * * 1`
- Every 5 minutes: `*/5 * * * *`

**Solution Steps:**
1. Create CronJobs with different schedules
2. Verify schedule syntax
3. Test schedule parsing

## Scenario 7.3: DaemonSets

### Question 7.3.1: Basic DaemonSet
Create a DaemonSet that:
- Name: `log-collector`
- Image: `fluent/fluentd:latest` (or `busybox:1.35` for testing)
- Runs on all nodes
- Has node selector if needed

**Solution Steps:**
1. Check `solutions/daemonset-log-collector.yaml`
2. Apply: `kubectl apply -f solutions/daemonset-log-collector.yaml`
3. Verify: `kubectl get daemonset log-collector`
4. Check pods: `kubectl get pods -l app=log-collector`
5. Should see one pod per node

### Question 7.3.2: DaemonSet with Node Selector
Create a DaemonSet that runs only on nodes with label `node-type=compute`:
- Name: `compute-monitor`

**Solution Steps:**
1. Check `solutions/daemonset-compute-monitor.yaml`
2. Apply and verify it only runs on labeled nodes

### Question 7.3.3: DaemonSet Rolling Update
Update a DaemonSet and observe rolling update:
- Update image version
- Observe pods update one by one
- Verify update strategy

**Solution Steps:**
1. Create DaemonSet
2. Update: `kubectl set image daemonset/log-collector fluentd=fluent/fluentd:v2.0`
3. Watch: `kubectl get pods -l app=log-collector -w`
4. Verify rolling update

### Question 7.3.4: DaemonSet with Tolerations
Create a DaemonSet that tolerates node taints:
- DaemonSet runs on all nodes including tainted ones
- Add appropriate tolerations

**Solution Steps:**
1. Taint a node: `kubectl taint nodes <node-name> node-role.kubernetes.io/master:NoSchedule`
2. Create DaemonSet with tolerations
3. Verify it runs on tainted node

### Question 7.3.5: DaemonSet Update Strategy
Create DaemonSet with different update strategies:
- OnDelete: Update only when pod is deleted
- RollingUpdate: Update pods one by one

**Solution Steps:**
1. Check `solutions/daemonset-update-strategy.yaml`
2. Apply and test update strategies
3. Understand when to use each

## Scenario 7.4: StatefulSets

### Question 7.4.1: Basic StatefulSet
Create a StatefulSet that:
- Name: `web-statefulset`
- Replicas: 3
- Image: `nginx:1.21`
- Service name: `web-headless`
- Each pod gets stable network identity

**Solution Steps:**
1. First create headless service (see `solutions/service-headless-statefulset.yaml`)
2. Check `solutions/statefulset-basic.yaml`
3. Apply service first, then StatefulSet
4. Verify: `kubectl get statefulset web-statefulset`
5. Check pod names: `kubectl get pods -l app=web` (should be web-statefulset-0, -1, -2)
6. Test DNS: `kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- nslookup web-statefulset-0.web-headless`

### Question 7.4.2: StatefulSet with Persistent Storage
Create a StatefulSet with persistent storage (covered in storage section).

### Question 7.4.3: StatefulSet Update Strategy
Create a StatefulSet with update strategy:
- RollingUpdate with partition
- Update specific replicas only
- Test canary deployment pattern

**Solution Steps:**
1. Create StatefulSet with 5 replicas
2. Set partition: `kubectl patch statefulset web-statefulset -p '{"spec":{"updateStrategy":{"type":"RollingUpdate","rollingUpdate":{"partition":3}}}}'`
3. Update image and verify only replicas 0-2 update
4. Reduce partition to update remaining replicas

### Question 7.4.4: StatefulSet Scaling
Scale StatefulSet up and down:
- Scale from 3 to 5 replicas
- Scale from 5 to 2 replicas
- Observe ordered scaling

**Solution Steps:**
1. Scale up: `kubectl scale statefulset web-statefulset --replicas=5`
2. Watch ordered creation
3. Scale down: `kubectl scale statefulset web-statefulset --replicas=2`
4. Watch ordered deletion (highest index first)

### Question 7.4.5: StatefulSet Pod Management
Understand StatefulSet pod management:
- Delete a pod and observe recreation
- Delete pod with higher index
- Verify pod identity is maintained

**Solution Steps:**
1. Delete pod: `kubectl delete pod web-statefulset-1`
2. Watch recreation: `kubectl get pods -w -l app=web`
3. Verify pod gets same name and identity
4. Check PVC is reused

## Scenario 7.5: Init Containers

### Question 7.5.1: Pod with Init Containers
Create a Pod with:
- Name: `app-with-init`
- Init container 1: Wait for service to be ready
- Init container 2: Download configuration
- Main container: Run application

**Solution Steps:**
1. Check `solutions/pod-init-containers.yaml`
2. Apply and watch init containers complete before main container starts
3. Verify: `kubectl describe pod app-with-init`

### Question 7.5.2: Init Container with Shared Volume
Create a Pod with init container that prepares data:
- Init container writes data to shared volume
- Main container reads from same volume
- Use emptyDir volume

**Solution Steps:**
1. Check `solutions/pod-init-shared-volume.yaml`
2. Apply and verify data sharing
3. Test: `kubectl exec <pod> -- cat /shared/data.txt`

### Question 7.5.3: Multiple Init Containers Order
Create Pod with multiple init containers:
- Understand execution order (sequential)
- Each init container must succeed
- If one fails, pod fails

**Solution Steps:**
1. Create pod with 3 init containers
2. Watch execution order
3. Make one fail and observe behavior

### Question 7.5.4: Init Container Resource Limits
Set resource limits on init containers:
- Init container with CPU/memory limits
- Different limits than main container
- Verify resource usage

**Solution Steps:**
1. Create pod with init container resources
2. Verify: `kubectl describe pod <pod-name> | grep -A 5 "Init Containers"`
3. Check resource allocation

## Scenario 7.6: Complex Multi-Container Scenarios

### Question 7.6.1: Sidecar Pattern
Create a Pod with sidecar container:
- Main container: Application
- Sidecar: Log collector or proxy
- Shared volume between containers

**Solution Steps:**
1. Check `solutions/pod-sidecar.yaml`
2. Apply and verify both containers run
3. Test sidecar functionality

### Question 7.6.2: Ambassador Pattern
Create Pod with ambassador container:
- Main container: Application
- Ambassador: Service proxy
- Ambassador handles external communication

**Solution Steps:**
1. Create pod with ambassador pattern
2. Verify ambassador routes traffic
3. Test external connectivity

### Question 7.6.3: Adapter Pattern
Create Pod with adapter container:
- Main container: Application
- Adapter: Transforms output format
- Shared volume for data exchange

**Solution Steps:**
1. Create pod with adapter pattern
2. Verify adapter transforms data
3. Test data flow

## Scenario 7.7: Workload Integration

### Question 7.7.1: Job Triggered by CronJob
Create a system where:
- CronJob creates Job
- Job processes data
- Job completion triggers cleanup

**Solution Steps:**
1. Create CronJob
2. Create Job template in CronJob
3. Verify Job creation and execution

### Question 7.7.2: DaemonSet with StatefulSet
Deploy system with both:
- StatefulSet: Database with persistent storage
- DaemonSet: Log collector on all nodes
- Both work together

**Solution Steps:**
1. Create StatefulSet
2. Create DaemonSet
3. Verify both run correctly
4. Test integration

### Question 7.7.3: Deployment with Job Cleanup
Create Deployment that:
- Runs application
- Creates Jobs for batch processing
- Cleans up completed Jobs

**Solution Steps:**
1. Create Deployment
2. Create Job with TTL
3. Verify automatic cleanup

## Practice Commands

```bash
# Jobs
kubectl get jobs
kubectl get job <job-name>
kubectl logs job/<job-name>
kubectl delete job <job-name>

# CronJobs
kubectl get cronjobs
kubectl get cronjob <cronjob-name>
kubectl create job --from=cronjob/<cronjob-name> <job-name>
kubectl delete cronjob <cronjob-name>

# DaemonSets
kubectl get daemonsets
kubectl get daemonset <ds-name>
kubectl describe daemonset <ds-name>
kubectl get pods -l app=<label>

# StatefulSets
kubectl get statefulsets
kubectl get statefulset <sts-name>
kubectl scale statefulset <sts-name> --replicas=5
kubectl delete statefulset <sts-name>

# Init containers
kubectl describe pod <pod-name> | grep -A 10 Init
kubectl logs <pod-name> -c <init-container-name>
```

