# CKA Practice Exam 9 - Advanced Patterns & Best Practices

**Duration**: 2 hours  
**Total Tasks**: 15  
**Passing Score**: 66% (74% for CKA)

---

## Task 1: Canary Deployment (15 minutes)

Implement canary deployment:
1. Stable deployment (5 replicas, nginx:1.21)
2. Canary deployment (1 replica, nginx:1.22)
3. Same service selects both
4. Monitor canary
5. Rollout or rollback decision

---

## Task 2: Blue-Green Deployment (15 minutes)

Implement blue-green pattern:
1. Blue deployment (current, 3 replicas)
2. Green deployment (new, 3 replicas)
3. Switch service between them
4. Rollback capability

---

## Task 3: Pod Anti-Affinity (12 minutes)

Create deployment with pod anti-affinity:
- 5 replicas
- Ensure pods on different nodes
- Use required anti-affinity
- Verify distribution

---

## Task 4: Pod Affinity (12 minutes)

Create deployments with pod affinity:
- Cache deployment (2 replicas)
- App deployment (3 replicas)
- App pods prefer co-location with cache
- Verify affinity works

---

## Task 5: Node Affinity (12 minutes)

Create pod with node affinity:
- Required: zone in [us-east-1a, us-east-1b]
- Preferred: node-type=compute (weight 100)
- Label nodes appropriately
- Verify scheduling

---

## Task 6: Priority Classes (12 minutes)

1. Create PriorityClass `high-priority` (value 1000)
2. Create PriorityClass `low-priority` (value 100)
3. Create pods with different priorities
4. Verify scheduling order

---

## Task 7: Init Containers (12 minutes)

Create pod with init containers:
- Init 1: Wait for service
- Init 2: Download config
- Main container: Application
- Verify execution order

---

## Task 8: Sidecar Pattern (12 minutes)

Create pod with sidecar:
- Main container: Application
- Sidecar: Log collector (tails logs)
- Shared volume
- Verify both containers run

---

## Task 9: ConfigMap Hot Reload (10 minutes)

1. Create ConfigMap and pod using it
2. Update ConfigMap
3. Check if pod picks up changes
4. Restart pod to force reload
5. Understand sync period

---

## Task 10: Immutable ConfigMap (8 minutes)

1. Create ConfigMap
2. Mark as immutable
3. Try to update (should fail)
4. Create new ConfigMap version

---

## Task 11: Projected Volumes (12 minutes)

Create pod with projected volume containing:
- ConfigMap
- Secret
- Downward API
- ServiceAccount token
- Mount and verify all

---

## Task 12: Downward API (10 minutes)

Create pod that uses Downward API to expose:
- Pod name
- Namespace
- Node name
- Pod IP
- As environment variables

---

## Task 13: Resource Quota Scopes (12 minutes)

Create ResourceQuota with scopes:
- `NotTerminating` scope
- Limits for non-terminating pods
- Test with different pod types
- Verify scope application

---

## Task 14: Complete Production Setup (20 minutes)

Deploy production-ready application:
1. Namespace with ResourceQuota and LimitRange
2. Deployment with health probes, resources, PDB
3. Service with session affinity
4. Ingress with TLS
5. NetworkPolicies
6. RBAC (least privilege)
7. Pod Security Standards
8. Monitoring labels

---

## Task 15: Multi-Tenant Setup (15 minutes)

Create multi-tenant environment:
1. Namespaces: tenant-a, tenant-b
2. ResourceQuotas per tenant
3. NetworkPolicies for isolation
4. RBAC per tenant
5. Verify isolation

---

## Scoring

- **Tasks 1-2**: 10 points each (20 points)
- **Tasks 3-8**: 7 points each (42 points)
- **Tasks 9-13**: 6 points each (30 points)
- **Tasks 14-15**: 4 points each (8 points)

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

**Good luck!** 🚀

