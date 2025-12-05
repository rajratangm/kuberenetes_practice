# CKA Practice Exam 5 - Security & RBAC Intensive

**Duration**: 2 hours  
**Total Tasks**: 17  
**Passing Score**: 66% (74% for CKA)

---

## Task 1: ServiceAccount Creation (6 minutes)

Create ServiceAccount `api-sa` in namespace `production` and use it in a deployment.

---

## Task 2: Basic Role and RoleBinding (10 minutes)

Create Role `pod-manager` that allows:
- `create`, `get`, `list`, `update`, `delete` on pods
- `get`, `list` on services

Bind to ServiceAccount `manager-sa` in namespace `development`.

---

## Task 3: ClusterRole and ClusterRoleBinding (12 minutes)

Create ClusterRole `node-viewer` that allows:
- `get`, `list`, `watch` on nodes
- `get`, `list` on namespaces

Bind to ServiceAccount `cluster-monitor` in namespace `monitoring`.

---

## Task 4: RBAC with Resource Names (10 minutes)

Create Role `specific-pod-role` that:
- Allows `get`, `update` on pods named `web-pod` and `api-pod`
- Allows `list` on all pods

Bind to ServiceAccount `operator-sa`.

---

## Task 5: Check Permissions (8 minutes)

Use `kubectl auth can-i` to:
- Check if current user can create pods
- Check if ServiceAccount `test-sa` can list pods
- List all permissions for ServiceAccount `test-sa`

---

## Task 6: Pod Security Context (12 minutes)

Create pod `secure-pod` that:
- Runs as non-root (UID 1000, GID 2000)
- Has read-only root filesystem
- Drops all capabilities
- Adds only `NET_BIND_SERVICE`
- Uses seccomp profile `RuntimeDefault`

---

## Task 7: Pod Security Standards (12 minutes)

1. Create namespace `restricted-ns` with `restricted` Pod Security Standard
2. Create pod that complies with restricted policy
3. Try to create non-compliant pod (should fail)

---

## Task 8: Secret Management (10 minutes)

1. Create secret `app-secret` with `api_key=abc123` and `token=xyz789`
2. Use in pod as environment variables
3. Use in pod as volume mount
4. Verify secret is base64 encoded

---

## Task 9: ImagePullSecrets (10 minutes)

1. Create docker-registry secret `regcred`
2. Create ServiceAccount `private-sa` with imagePullSecrets
3. Create pod using this ServiceAccount to pull private image

---

## Task 10: NetworkPolicy Default Deny (12 minutes)

In namespace `secure`:
1. Create default deny-all NetworkPolicy
2. Allow specific ingress traffic
3. Allow specific egress traffic
4. Test connectivity

---

## Task 11: NetworkPolicy Namespace Isolation (12 minutes)

Create NetworkPolicy that:
- Allows pods in `frontend` namespace to access pods in `backend` namespace
- Denies all other cross-namespace traffic
- Uses namespaceSelector

---

## Task 12: RBAC Aggregation (10 minutes)

Create ClusterRole with aggregation labels that:
- Aggregates permissions from multiple ClusterRoles
- Allows viewing pods and services
- Test aggregated permissions

---

## Task 13: Role with Subresources (10 minutes)

Create Role that allows:
- `get`, `list` on pods
- `get` on pods/status
- `get`, `list` on pods/log

Bind to ServiceAccount and verify.

---

## Task 14: ServiceAccount in Deployment (8 minutes)

Create deployment that:
- Uses custom ServiceAccount
- ServiceAccount has specific RBAC permissions
- Verify deployment pods use the ServiceAccount

---

## Task 15: TLS Secret (10 minutes)

Create TLS secret `tls-secret` with:
- Certificate and key (you can use self-signed for practice)
- Use in Ingress resource
- Verify TLS configuration

---

## Task 16: Pod with Security Context Inheritance (10 minutes)

Create pod where:
- Pod-level security context sets runAsUser
- Container-level overrides with different user
- Understand precedence
- Verify both contexts

---

## Task 17: Complete Security Setup (12 minutes)

In namespace `secure-app`:
1. Create ResourceQuota
2. Create LimitRange
3. Apply Pod Security Standard `restricted`
4. Create NetworkPolicy (default deny)
5. Create RBAC (least privilege)
6. Deploy secure application

---

## Scoring

- **Tasks 1-5**: 6 points each (30 points)
- **Tasks 6-10**: 7 points each (35 points)
- **Tasks 11-15**: 6 points each (30 points)
- **Tasks 16-17**: 5 points each (10 points)

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

**Good luck!** 🚀

