# CKA Practice Exam 7 - Networking & Ingress

**Duration**: 2 hours  
**Total Tasks**: 17  
**Passing Score**: 66% (74% for CKA)

---

## Task 1: ClusterIP Service (8 minutes)

Create ClusterIP service `app-service` that:
- Selects pods with label `app=web`
- Exposes port 80
- Maps to container port 8080
- Verify endpoints

---

## Task 2: NodePort Service (8 minutes)

Create NodePort service `web-nodeport` that:
- Selects pods with label `app=web`
- Exposes port 80
- NodePort: 30080
- Test accessibility

---

## Task 3: LoadBalancer Service (8 minutes)

Create LoadBalancer service `app-lb` that:
- Selects pods with label `app=api`
- Exposes port 8080
- Verify external IP assignment (if supported)

---

## Task 4: Headless Service (10 minutes)

Create headless service `db-headless` that:
- ClusterIP: None
- Selects pods with label `app=database`
- Port 3306
- Test DNS resolution (returns all pod IPs)

---

## Task 5: Service Session Affinity (10 minutes)

Create service with:
- Session affinity: ClientIP
- Timeout: 3 hours
- Verify sticky sessions work

---

## Task 6: Service Endpoints (10 minutes)

1. Create service with no selector
2. Manually create Endpoints resource
3. Point to external IP
4. Test connectivity

---

## Task 7: Basic Ingress (10 minutes)

Create Ingress `web-ingress` that:
- Uses ingress class `nginx`
- Routes host `app.example.com` to service `web-service:80`
- Verify routing

---

## Task 8: Ingress Multiple Paths (12 minutes)

Create Ingress with multiple paths:
- `/api` → `api-service:80`
- `/web` → `web-service:80`
- `/admin` → `admin-service:80`
- Default `/` → `web-service:80`

---

## Task 9: Ingress TLS (12 minutes)

Create Ingress with TLS:
- Host: `secure.example.com`
- TLS secret: `tls-secret`
- Routes to `web-service:80`
- Verify TLS configuration

---

## Task 10: Ingress Default Backend (10 minutes)

Create Ingress with:
- Default backend service
- Custom 404 handler
- Verify default backend works

---

## Task 11: Ingress Rewrite (12 minutes)

Create Ingress with path rewrite:
- Incoming: `/api/v1/users`
- Rewrite to: `/users`
- Backend: `api-service:8080`
- Use annotations for rewrite

---

## Task 12: NetworkPolicy Default Deny (10 minutes)

Create NetworkPolicy that:
- Applies to all pods in namespace
- Denies all ingress
- Denies all egress
- Test isolation

---

## Task 13: NetworkPolicy Allow Specific (12 minutes)

Create NetworkPolicy that:
- Allows pods with label `app=web` to receive on port 80
- Allows pods with label `app=web` to send to `app=api` on port 8080
- Denies all other traffic

---

## Task 14: NetworkPolicy with IP Block (12 minutes)

Create NetworkPolicy that:
- Allows ingress from IP range `10.0.0.0/8`
- Allows ingress from pods with specific label
- Denies all other ingress

---

## Task 15: NetworkPolicy Egress Rules (12 minutes)

Create NetworkPolicy with:
- Specific egress rules
- Allow DNS (port 53)
- Allow specific service communication
- Deny all other egress

---

## Task 16: Cross-Namespace Networking (12 minutes)

Create NetworkPolicy that:
- Allows pods in `frontend` namespace to access `backend` namespace
- Uses namespaceSelector
- Test cross-namespace connectivity

---

## Task 17: Complete Networking Setup (12 minutes)

Deploy complete networking:
1. Frontend service (ClusterIP)
2. Backend service (ClusterIP)
3. Database service (Headless)
4. Ingress routing frontend
5. NetworkPolicies for isolation
6. Test end-to-end connectivity

---

## Scoring

- **Tasks 1-6**: 5 points each (30 points)
- **Tasks 7-11**: 6 points each (30 points)
- **Tasks 12-16**: 7 points each (35 points)
- **Task 17**: 5 points

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

**Good luck!** 🚀

