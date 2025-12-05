# CKA Practice Exam Scenarios

This document contains comprehensive practice exam scenarios that simulate real CKA exam questions.

## Practice Exam 1: Complete Application Setup

### Scenario
You need to deploy a complete web application with the following requirements:

1. **Deployment**: Create a deployment named `web-app` with:
   - 3 replicas
   - Image: `nginx:1.21`
   - Labels: `app=web`, `tier=frontend`
   - Resource requests: CPU 100m, Memory 128Mi
   - Resource limits: CPU 500m, Memory 512Mi
   - Readiness probe on port 80
   - Liveness probe on port 80

2. **Service**: Create a ClusterIP service named `web-service` that:
   - Exposes the deployment on port 80
   - Selector: `app=web`, `tier=frontend`

3. **ConfigMap**: Create a ConfigMap named `app-config` with:
   - `server_name`: `myapp.example.com`
   - `log_level`: `info`

4. **Secret**: Create a secret named `app-secret` with:
   - `api_key`: `secret12345`
   - `db_password`: `mypassword`

5. **Update Deployment**: Update the deployment to:
   - Use the ConfigMap as environment variables
   - Use the Secret as environment variables
   - Mount the ConfigMap as a volume at `/etc/config`

**Time Limit**: 15 minutes

**Solution Steps**:
1. Create deployment: `kubectl apply -f 01-core-concepts/solutions/deployment-frontend.yaml`
2. Create service: `kubectl apply -f 02-networking/solutions/service-clusterip.yaml`
3. Create ConfigMap: `kubectl create configmap app-config --from-literal=server_name=myapp.example.com --from-literal=log_level=info`
4. Create Secret: `kubectl create secret generic app-secret --from-literal=api_key=secret12345 --from-literal=db_password=mypassword`
5. Update deployment with env vars and volume mounts

## Practice Exam 2: Networking and Ingress

### Scenario
Set up networking for a multi-tier application:

1. **Backend Deployment**: Create deployment `api-backend` with:
   - 2 replicas
   - Image: `httpd:2.4`
   - Labels: `app=api`, `tier=backend`

2. **Frontend Deployment**: Create deployment `web-frontend` with:
   - 3 replicas
   - Image: `nginx:1.21`
   - Labels: `app=web`, `tier=frontend`

3. **Backend Service**: Create ClusterIP service `api-service` on port 8080

4. **Frontend Service**: Create ClusterIP service `web-service` on port 80

5. **Ingress**: Create an Ingress that:
   - Routes `/api/*` to `api-service:8080`
   - Routes `/*` to `web-service:80`
   - Host: `app.example.com`

**Time Limit**: 12 minutes

**Solution Steps**:
1. Create both deployments
2. Create both services
3. Create ingress with multiple paths (see `02-networking/solutions/ingress-multi-path.yaml`)

## Practice Exam 3: Storage and StatefulSets

### Scenario
Deploy a stateful application with persistent storage:

1. **StorageClass**: Create a StorageClass named `fast-ssd` with:
   - Provisioner: `kubernetes.io/no-provisioner`
   - Volume binding mode: `WaitForFirstConsumer`

2. **StatefulSet**: Create a StatefulSet named `db-statefulset` with:
   - 3 replicas
   - Image: `nginx:1.21` (for testing)
   - Service name: `db-headless`
   - Each pod should have a PVC of 5Gi using `fast-ssd` StorageClass
   - Mount the volume at `/data`

3. **Headless Service**: Create a headless service `db-headless` for the StatefulSet

**Time Limit**: 10 minutes

**Solution Steps**:
1. Create StorageClass (see `03-storage/solutions/storageclass-fast-ssd.yaml`)
2. Create headless service (see `02-networking/solutions/service-headless.yaml`)
3. Create StatefulSet with volumeClaimTemplates (see `03-storage/solutions/statefulset-with-storage.yaml`)

## Practice Exam 4: Security and RBAC

### Scenario
Set up RBAC for a microservice:

1. **Namespace**: Create namespace `production`

2. **ServiceAccount**: Create ServiceAccount `api-sa` in `production` namespace

3. **Role**: Create a Role `pod-reader` in `production` that allows:
   - Get, list, watch pods
   - Get, list, watch services

4. **RoleBinding**: Bind `pod-reader` role to `api-sa` ServiceAccount

5. **Pod**: Create a pod that uses `api-sa` ServiceAccount

6. **Secret**: Create a secret `db-credentials` with username and password

7. **Pod with Secret**: Create a pod that uses the secret as environment variables

**Time Limit**: 12 minutes

**Solution Steps**:
1. Create namespace: `kubectl create namespace production`
2. Create ServiceAccount (see `04-security/solutions/serviceaccount-api.yaml`)
3. Create Role (see `04-security/solutions/role-pod-manager.yaml`)
4. Create RoleBinding (see `04-security/solutions/rolebinding-pod-manager.yaml`)
5. Create pod with ServiceAccount (see `04-security/solutions/pod-with-serviceaccount.yaml`)
6. Create secret and pod (see `04-security/solutions/secret-app.yaml` and `04-security/solutions/pod-secret-env.yaml`)

## Practice Exam 5: Troubleshooting

### Scenario
Diagnose and fix the following issues:

1. **Issue 1**: Pod `broken-pod` is in CrashLoopBackOff
   - Find the issue and fix it
   - Pod should run successfully

2. **Issue 2**: Service `web-service` has no endpoints
   - Diagnose why
   - Fix the issue

3. **Issue 3**: Deployment `app-deployment` rollout is stuck
   - Find the problem
   - Fix and verify rollout completes

**Time Limit**: 15 minutes

**Solution Steps**:
1. Check logs: `kubectl logs broken-pod --previous`
2. Fix the issue (wrong image, command, etc.)
3. Check service selector vs pod labels
4. Check deployment events and fix image/configuration issues

## Practice Exam 6: Advanced Workloads

### Scenario
Set up various workload types:

1. **Job**: Create a Job `data-processor` that:
   - Runs a command: `echo "Processing complete" && sleep 30`
   - Image: `busybox:1.35`

2. **CronJob**: Create a CronJob `backup-job` that:
   - Runs every day at 2 AM
   - Command: `echo "Backup done" && date`

3. **DaemonSet**: Create a DaemonSet `log-collector` that:
   - Runs on all nodes
   - Image: `busybox:1.35`
   - Command: `sleep 3600`

4. **Deployment with Anti-Affinity**: Create a deployment `web-distributed` with:
   - 3 replicas
   - Pod anti-affinity to ensure pods are on different nodes
   - Image: `nginx:1.21`

**Time Limit**: 15 minutes

**Solution Steps**:
1. Create Job (see `07-advanced-workloads/solutions/job-data-processor.yaml`)
2. Create CronJob (see `07-advanced-workloads/solutions/cronjob-backup.yaml`)
3. Create DaemonSet (see `07-advanced-workloads/solutions/daemonset-log-collector.yaml`)
4. Create Deployment with anti-affinity (see `08-configuration/solutions/deployment-pod-antiaffinity.yaml`)

## Practice Exam 7: Cluster Maintenance

### Scenario
Perform cluster maintenance tasks:

1. **Node Management**:
   - Label node `worker-1` with `node-type=compute`
   - Taint the node with `dedicated=special:NoSchedule`
   - Cordon the node
   - Create a pod with toleration that can run on the tainted node

2. **Resource Quota**: Create a ResourceQuota in namespace `production`:
   - CPU requests: 2
   - CPU limits: 4
   - Memory requests: 4Gi
   - Memory limits: 8Gi
   - Pods: 10

3. **LimitRange**: Create a LimitRange in namespace `production`:
   - Default CPU request: 100m
   - Default CPU limit: 200m
   - Default memory request: 128Mi
   - Default memory limit: 256Mi

4. **Export Resources**: Export all resources from namespace `production` to a file

**Time Limit**: 12 minutes

**Solution Steps**:
1. Label: `kubectl label nodes worker-1 node-type=compute`
2. Taint: `kubectl taint nodes worker-1 dedicated=special:NoSchedule`
3. Cordon: `kubectl cordon worker-1`
4. Create pod with toleration (see `08-configuration/solutions/pod-with-toleration.yaml`)
5. Create ResourceQuota (see `06-cluster-maintenance/solutions/resourcequota-example.yaml`)
6. Create LimitRange (see `06-cluster-maintenance/solutions/limitrange-example.yaml`)
7. Export: `kubectl get all,configmap,secret -n production -o yaml > backup.yaml`

## Practice Exam 8: Complete Multi-Tier Application

### Scenario
Deploy a complete 3-tier application:

1. **Database Layer**:
   - StatefulSet `mysql-statefulset` with 1 replica
   - Persistent storage (5Gi)
   - Headless service `mysql-headless`

2. **Application Layer**:
   - Deployment `app-backend` with 2 replicas
   - Image: `httpd:2.4`
   - Environment variables from ConfigMap and Secret
   - Resource limits

3. **Web Layer**:
   - Deployment `app-frontend` with 3 replicas
   - Image: `nginx:1.21`
   - Service `frontend-service` (ClusterIP)

4. **Ingress**: Route traffic to frontend service

5. **NetworkPolicy**: Allow only frontend pods to access backend pods

**Time Limit**: 20 minutes

**Solution Steps**:
1. Create StatefulSet with storage
2. Create backend deployment with ConfigMap and Secret
3. Create frontend deployment and service
4. Create Ingress
5. Create NetworkPolicy (see `02-networking/solutions/networkpolicy-allow-specific.yaml`)

## Exam Simulation Tips

1. **Read questions carefully** - Understand all requirements
2. **Time management** - Don't spend too long on one question
3. **Verify your work** - Always check if resources are created correctly
4. **Use kubectl explain** - When unsure about YAML structure
5. **Test connectivity** - Verify services and networking work
6. **Check events** - Use `kubectl get events` for troubleshooting
7. **Clean up** - Delete test resources if needed to avoid confusion

## Scoring Yourself

- **90-100%**: Excellent! You're ready for the exam
- **70-89%**: Good, but review weak areas
- **Below 70%**: Focus on fundamentals and practice more

Good luck with your CKA exam preparation!

