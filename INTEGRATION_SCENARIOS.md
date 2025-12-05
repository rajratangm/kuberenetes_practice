# Integration Scenarios - Complex Multi-Step CKA Questions

These scenarios combine multiple Kubernetes concepts and are designed to simulate real-world CKA exam questions.

## Integration Scenario 1: Complete Web Application Stack

### Requirements
Deploy a complete 3-tier web application with:

1. **Database Layer (StatefulSet)**
   - StatefulSet: `mysql-db` with 3 replicas
   - Headless service: `mysql-headless`
   - Persistent storage: 10Gi per pod
   - StorageClass: `fast-ssd`
   - ConfigMap: Database configuration
   - Secret: Database credentials

2. **Application Layer (Deployment)**
   - Deployment: `app-backend` with 5 replicas
   - Image: `httpd:2.4`
   - Service: `app-service` (ClusterIP) on port 8080
   - Environment variables from ConfigMap and Secret
   - Resource limits: CPU 500m, Memory 512Mi
   - Readiness and liveness probes
   - Pod anti-affinity (pods on different nodes)

3. **Web Layer (Deployment)**
   - Deployment: `web-frontend` with 3 replicas
   - Image: `nginx:1.21`
   - Service: `web-service` (ClusterIP) on port 80
   - ConfigMap: Nginx configuration
   - Ingress: Routes traffic to web-service

4. **Security**
   - ServiceAccount: `app-sa`
   - Role: Allows get/list pods and services
   - RoleBinding: Binds role to ServiceAccount
   - NetworkPolicy: Allow only frontend to access backend

5. **High Availability**
   - PodDisruptionBudget: Min 2 pods for frontend
   - ResourceQuota: CPU 10, Memory 20Gi for namespace

**Time Limit**: 25 minutes

**Solution Steps**:
1. Create namespace: `kubectl create namespace production`
2. Create StorageClass
3. Create ConfigMaps and Secrets
4. Create StatefulSet with headless service
5. Create backend deployment and service
6. Create frontend deployment and service
7. Create Ingress
8. Create RBAC resources
9. Create NetworkPolicy
10. Create PDB and ResourceQuota
11. Verify all components work together

## Integration Scenario 2: Microservices with Service Mesh Concepts

### Requirements
Deploy microservices architecture:

1. **Service A (Deployment)**
   - 3 replicas
   - Service: `service-a`
   - ConfigMap: Service configuration
   - Exposes metrics endpoint

2. **Service B (Deployment)**
   - 2 replicas
   - Service: `service-b`
   - Calls Service A
   - Uses ServiceAccount with permissions

3. **Service C (StatefulSet)**
   - 2 replicas
   - Stateful service with persistent storage
   - Headless service for direct pod access

4. **Ingress**
   - Routes `/api/v1/*` to Service A
   - Routes `/api/v2/*` to Service B
   - TLS termination

5. **Network Policies**
   - Service A: Allow ingress from Ingress controller
   - Service B: Allow ingress from Service A only
   - Service C: Allow ingress from Service B only

6. **Monitoring (DaemonSet)**
   - Log collector on all nodes
   - Collects logs from all services

**Time Limit**: 20 minutes

## Integration Scenario 3: Batch Processing System

### Requirements
Create batch processing system:

1. **Data Ingestion (CronJob)**
   - Runs every hour
   - Creates Job to ingest data
   - Job processes files and stores in PVC

2. **Processing Jobs**
   - Job template with parallelism
   - Processes data from shared PVC
   - TTL: 1 hour after completion

3. **Storage**
   - PVC: 50Gi for data storage
   - Shared by ingestion and processing

4. **Monitoring**
   - Job completion tracking
   - Resource usage monitoring

**Time Limit**: 15 minutes

## Integration Scenario 4: Multi-Namespace Application

### Requirements
Deploy application across namespaces:

1. **Namespace: development**
   - Deployment: `dev-app` (2 replicas)
   - Service: `dev-service`
   - ResourceQuota: CPU 2, Memory 4Gi

2. **Namespace: staging**
   - Deployment: `staging-app` (3 replicas)
   - Service: `staging-service`
   - Same image as dev but different config

3. **Namespace: production**
   - Deployment: `prod-app` (5 replicas)
   - Service: `prod-service`
   - PDB: Min 3 available
   - ResourceQuota: CPU 10, Memory 20Gi

4. **Cross-Namespace Access**
   - Service in production accesses staging service
   - Use FQDN for cross-namespace service discovery

5. **RBAC**
   - ClusterRole: View all namespaces
   - ClusterRoleBinding: Bind to ServiceAccount in production

**Time Limit**: 18 minutes

## Integration Scenario 5: Disaster Recovery Setup

### Requirements
Set up disaster recovery:

1. **Backup CronJob**
   - Runs daily at 2 AM
   - Backs up all ConfigMaps and Secrets
   - Stores backup in PVC

2. **Resource Export**
   - Export all deployments
   - Export all services
   - Export all ConfigMaps and Secrets
   - Create backup manifest

3. **Verification**
   - Verify backup integrity
   - Test restore process
   - Document restore procedure

**Time Limit**: 15 minutes

## Integration Scenario 6: High-Performance Computing

### Requirements
Deploy HPC workload:

1. **Compute Nodes**
   - Label nodes: `compute-type=gpu`
   - Taint nodes: `gpu=true:NoSchedule`

2. **GPU Workload (Job)**
   - Job with 10 completions
   - Parallelism: 5
   - Tolerates GPU taint
   - Node affinity to GPU nodes
   - Resource requests: CPU 4, Memory 8Gi

3. **Data Layer**
   - StatefulSet: Data storage
   - Shared PVC for input/output

4. **Monitoring**
   - DaemonSet: Collects metrics
   - Runs on all nodes including GPU nodes

**Time Limit**: 20 minutes

## Integration Scenario 7: Canary Deployment

### Requirements
Implement canary deployment:

1. **Stable Version (Deployment)**
   - Deployment: `app-stable` (5 replicas)
   - Image: `app:v1.0`
   - Service: `app-service` (selects both stable and canary)

2. **Canary Version (Deployment)**
   - Deployment: `app-canary` (1 replica)
   - Image: `app:v1.1`
   - Same labels as stable (for service)

3. **Traffic Splitting**
   - Use service to route 10% to canary
   - Monitor canary performance

4. **Rollback Capability**
   - Maintain deployment history
   - Quick rollback if needed

**Time Limit**: 15 minutes

## Integration Scenario 8: Multi-Region Deployment

### Requirements
Deploy across multiple zones:

1. **Zone A Deployment**
   - Deployment: `app-zone-a` (3 replicas)
   - Node affinity: `zone=us-east-1a`
   - Service: `app-zone-a-service`

2. **Zone B Deployment**
   - Deployment: `app-zone-b` (3 replicas)
   - Node affinity: `zone=us-east-1b`
   - Service: `app-zone-b-service`

3. **Global Service**
   - Service: `app-global`
   - Routes to both zone services
   - ExternalName or multiple endpoints

4. **High Availability**
   - Pod anti-affinity across zones
   - PDB ensures availability per zone

**Time Limit**: 18 minutes

## Tips for Integration Scenarios

1. **Read All Requirements First**: Understand the complete picture
2. **Plan Your Approach**: Break down into logical steps
3. **Create Resources in Order**: Dependencies first (namespaces, ConfigMaps, Secrets)
4. **Verify as You Go**: Don't wait until the end to test
5. **Use Labels Consistently**: Makes selectors work correctly
6. **Test Connectivity**: Verify services can reach each other
7. **Check Resource Limits**: Ensure quotas don't block you
8. **Document Your Work**: Helps with verification

## Practice Strategy

1. **Start Simple**: Begin with basic integration scenarios
2. **Add Complexity**: Gradually add more components
3. **Time Yourself**: Practice under exam conditions
4. **Review Mistakes**: Understand what went wrong
5. **Repeat**: Do scenarios multiple times until perfect

Good luck with your CKA exam preparation! 🚀

