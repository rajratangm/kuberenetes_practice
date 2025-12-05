# Scenario 2: Networking

## Scenario 2.1: Services

### Question 2.1.1: ClusterIP Service
Create a ClusterIP Service named `web-service` that:
- Exposes port 80
- Targets pods with label `app=web`
- Maps service port 80 to pod port 80
- Has selector: `app=web`, `tier=frontend`

**Solution Steps:**
1. Ensure you have pods with matching labels (create a deployment first)
2. Check `solutions/service-clusterip.yaml`
3. Apply: `kubectl apply -f solutions/service-clusterip.yaml`
4. Verify: `kubectl get svc web-service`
5. Test: `kubectl run test-pod --image=busybox:1.35 --rm -it --restart=Never -- wget -O- http://web-service.default.svc.cluster.local`

### Question 2.1.2: NodePort Service
Create a NodePort Service named `web-nodeport` that:
- Exposes port 80
- NodePort should be 30080
- Targets pods with label `app=web`
- Maps service port 80 to pod port 80

**Solution Steps:**
1. Check `solutions/service-nodeport.yaml`
2. Apply and verify
3. Access via `<node-ip>:30080`

### Question 2.1.3: LoadBalancer Service
Create a LoadBalancer Service named `web-lb` that:
- Exposes port 80
- Targets pods with label `app=web`
- Has external traffic policy: Local

**Solution Steps:**
1. Check `solutions/service-loadbalancer.yaml`
2. Apply and verify (Note: LoadBalancer works in cloud environments)

### Question 2.1.4: Headless Service
Create a Headless Service (ClusterIP: None) named `db-headless` that:
- Targets pods with label `app=database`
- Used for StatefulSets or service discovery

**Solution Steps:**
1. Check `solutions/service-headless.yaml`
2. Apply and verify
3. Test DNS: `kubectl run test-pod --image=busybox:1.35 --rm -it --restart=Never -- nslookup db-headless.default.svc.cluster.local`

## Scenario 2.2: Ingress

### Question 2.2.1: Basic Ingress
Create an Ingress resource named `web-ingress` that:
- Routes traffic to service `web-service` on port 80
- Host: `example.com`
- Path: `/`
- Uses default ingress class

**Solution Steps:**
1. Ensure an Ingress Controller is installed (nginx, traefik, etc.)
2. Check `solutions/ingress-basic.yaml`
3. Apply: `kubectl apply -f solutions/ingress-basic.yaml`
4. Verify: `kubectl get ingress web-ingress`
5. Test: Add `/etc/hosts` entry or use curl with Host header

### Question 2.2.2: Ingress with Multiple Paths
Create an Ingress with:
- Name: `multi-path-ingress`
- Host: `app.example.com`
- Path `/api` routes to `api-service:8080`
- Path `/web` routes to `web-service:80`
- Default backend to `web-service:80`

**Solution Steps:**
1. Check `solutions/ingress-multi-path.yaml`
2. Apply and verify

### Question 2.2.3: Ingress with TLS
Create an Ingress with TLS:
- Name: `tls-ingress`
- Host: `secure.example.com`
- TLS secret: `tls-secret`
- Routes to `web-service:80`

**Solution Steps:**
1. First create TLS secret (see `solutions/secret-tls.yaml`)
2. Check `solutions/ingress-tls.yaml`
3. Apply both files

## Scenario 2.3: NetworkPolicies

### Question 2.3.1: Deny All Traffic
Create a NetworkPolicy named `deny-all` in namespace `default` that:
- Denies all ingress and egress traffic
- Applies to all pods in the namespace

**Solution Steps:**
1. Check `solutions/networkpolicy-deny-all.yaml`
2. Apply and verify
3. Test connectivity between pods

### Question 2.3.2: Allow Specific Pods
Create a NetworkPolicy that:
- Name: `allow-web`
- Allows ingress only from pods with label `role=frontend`
- Allows ingress on port 80
- Applies to pods with label `app=web`

**Solution Steps:**
1. Check `solutions/networkpolicy-allow-specific.yaml`
2. Apply and verify

### Question 2.3.3: Allow Namespace Traffic
Create a NetworkPolicy that:
- Name: `allow-namespace`
- Allows ingress from all pods in namespace `monitoring`
- Applies to pods with label `app=api`

**Solution Steps:**
1. Check `solutions/networkpolicy-allow-namespace.yaml`
2. Apply and verify

## Scenario 2.4: DNS and Service Discovery

### Question 2.4.1: Service DNS Resolution
Understand and test:
- FQDN: `<service-name>.<namespace>.svc.cluster.local`
- Short name: `<service-name>`
- Namespace-scoped: `<service-name>.<namespace>`

**Solution Steps:**
1. Create a test pod: `kubectl run dns-test --image=busybox:1.35 --rm -it --restart=Never -- nslookup web-service.default.svc.cluster.local`
2. Test short names within same namespace

### Question 2.1.5: Service with Multiple Ports
Create a Service that exposes multiple ports:
- Name: `multi-port-service`
- Port 80 → targetPort 8080 (HTTP)
- Port 443 → targetPort 8443 (HTTPS)
- Selector: `app=web`

**Solution Steps:**
1. Check `solutions/service-multi-port.yaml`
2. Apply and verify both ports
3. Test: `kubectl get svc multi-port-service -o yaml`

### Question 2.1.6: Service with Session Affinity
Create a Service with session affinity:
- Name: `sticky-service`
- Type: ClusterIP
- Session affinity: ClientIP
- Session affinity timeout: 10800 seconds

**Solution Steps:**
1. Check `solutions/service-session-affinity.yaml`
2. Apply and verify session affinity setting

### Question 2.1.7: Service Endpoints
Create a Service and verify endpoints:
- Service: `endpoint-service`
- Selector: `app=web`
- Create pods with matching labels
- Verify endpoints are created automatically

**Solution Steps:**
1. Create deployment with label `app=web`
2. Create service with selector `app=web`
3. Verify: `kubectl get endpoints endpoint-service`
4. Check: `kubectl describe svc endpoint-service`

### Question 2.1.8: ExternalName Service
Create an ExternalName service:
- Name: `external-db`
- External name: `database.example.com`
- Used to reference external services

**Solution Steps:**
1. Check `solutions/service-externalname.yaml`
2. Apply and verify
3. Test DNS: `kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- nslookup external-db`

### Question 2.1.9: Service without Selector
Create a Service without selector and manually create endpoints:
- Service: `manual-endpoint-service`
- Port: 3306
- Manually create Endpoints object pointing to external IP

**Solution Steps:**
1. Check `solutions/service-no-selector.yaml` and `solutions/endpoints-manual.yaml`
2. Apply both
3. Verify endpoints are manually managed

## Scenario 2.2: Ingress

### Question 2.2.4: Ingress with Default Backend
Create an Ingress with default backend:
- Name: `default-backend-ingress`
- Default backend to `default-service:80`
- All unmatched paths go to default

**Solution Steps:**
1. Check `solutions/ingress-default-backend.yaml`
2. Apply and verify default routing

### Question 2.2.5: Ingress with Rewrite Rules
Create an Ingress with path rewriting:
- Name: `rewrite-ingress`
- Host: `app.example.com`
- Path `/api/v1/*` rewrites to `/api/*` on backend
- Requires ingress controller with rewrite support

**Solution Steps:**
1. Check `solutions/ingress-rewrite.yaml`
2. Apply and test path rewriting

### Question 2.2.6: Ingress with Annotations
Create an Ingress with annotations:
- Name: `annotated-ingress`
- SSL redirect annotation
- Rate limiting annotation
- CORS annotation

**Solution Steps:**
1. Check `solutions/ingress-annotations.yaml`
2. Apply and verify annotations
3. Note: Annotations depend on ingress controller

### Question 2.2.7: Multiple Ingress Resources
Create multiple Ingress resources for same host:
- Ingress 1: Routes `/api/*` to `api-service`
- Ingress 2: Routes `/web/*` to `web-service`
- Both use host `app.example.com`

**Solution Steps:**
1. Create both ingress resources
2. Verify both are active
3. Test routing to both services

## Scenario 2.3: NetworkPolicies

### Question 2.3.4: NetworkPolicy with Egress Rules
Create a NetworkPolicy that controls egress:
- Name: `egress-policy`
- Allows egress only to pods with label `app=database` on port 3306
- Denies all other egress

**Solution Steps:**
1. Check `solutions/networkpolicy-egress.yaml`
2. Apply and verify egress restrictions

### Question 2.3.5: NetworkPolicy with IP Blocks
Create a NetworkPolicy using IP blocks:
- Name: `ip-block-policy`
- Allows ingress only from IP range `10.0.0.0/8`
- Applies to pods with label `app=api`

**Solution Steps:**
1. Check `solutions/networkpolicy-ipblock.yaml`
2. Apply and verify IP-based rules

### Question 2.3.6: NetworkPolicy with Port Ranges
Create a NetworkPolicy with specific ports:
- Name: `port-policy`
- Allows ingress on ports 80, 443, 8080-8090
- Applies to pods with label `app=web`

**Solution Steps:**
1. Check `solutions/networkpolicy-ports.yaml`
2. Apply and verify port restrictions

### Question 2.3.7: Multiple NetworkPolicies
Create multiple NetworkPolicies and understand how they combine:
- Policy 1: Allow from namespace `monitoring`
- Policy 2: Allow on port 80
- Both apply to same pods

**Solution Steps:**
1. Create both policies
2. Understand policies are additive (OR logic)
3. Test connectivity

### Question 2.3.8: NetworkPolicy Default Deny
Create a default deny policy, then allow specific traffic:
- Step 1: Deny all ingress and egress
- Step 2: Allow specific traffic patterns
- Verify isolation

**Solution Steps:**
1. Apply deny-all policy first
2. Then apply allow policies
3. Test connectivity step by step

## Scenario 2.4: DNS and Service Discovery

### Question 2.4.2: Cross-Namespace Service Discovery
Test service discovery across namespaces:
- Create service `web-service` in namespace `production`
- Access from pod in namespace `default`
- Use FQDN: `web-service.production.svc.cluster.local`

**Solution Steps:**
1. Create service in production namespace
2. Create pod in default namespace
3. Test: `kubectl exec <pod> -- nslookup web-service.production.svc.cluster.local`

### Question 2.4.3: Headless Service DNS
Create headless service and test DNS resolution:
- Headless service: `db-headless`
- StatefulSet with 3 replicas
- Test DNS returns all pod IPs

**Solution Steps:**
1. Create headless service
2. Create StatefulSet
3. Test: `kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- nslookup db-headless`

### Question 2.4.4: Service DNS Patterns
Understand and test all DNS patterns:
- Short name: `<service-name>`
- Namespace: `<service-name>.<namespace>`
- FQDN: `<service-name>.<namespace>.svc.cluster.local`
- Pod DNS in StatefulSet: `<pod-name>.<service-name>.<namespace>.svc.cluster.local`

**Solution Steps:**
1. Create services and pods
2. Test each DNS pattern
3. Document which works in which context

## Scenario 2.5: Service Mesh Basics (Conceptual)

### Question 2.5.1: Understand Service Mesh
Understand concepts (no implementation needed for CKA):
- What is a service mesh
- How it differs from regular services
- Common use cases

**Solution Steps:**
1. Research service mesh concepts
2. Understand Istio, Linkerd basics
3. Note: Not required for CKA but good to know

## Practice Commands

```bash
# Get services
kubectl get svc
kubectl get svc -A
kubectl get svc -o wide

# Describe service
kubectl describe svc <service-name>
kubectl get svc <service-name> -o yaml

# Get endpoints
kubectl get endpoints <service-name>
kubectl get endpoints -A
kubectl describe endpoints <service-name>

# Port forward
kubectl port-forward svc/<service-name> 8080:80
kubectl port-forward pod/<pod-name> 8080:80

# Get ingress
kubectl get ingress
kubectl get ingress -A
kubectl describe ingress <ingress-name>
kubectl get ingress <ingress-name> -o yaml

# Test DNS
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- nslookup <service-name>
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- wget -O- http://<service-name>

# NetworkPolicy
kubectl get networkpolicy
kubectl get networkpolicy -A
kubectl describe networkpolicy <policy-name>
kubectl get networkpolicy <policy-name> -o yaml

# Test connectivity
kubectl run test --image=busybox:1.35 --rm -it --restart=Never -- wget -O- http://<service-name>:<port>
kubectl exec <pod-name> -- curl http://<service-name>

# Service endpoints
kubectl get endpoints
kubectl patch svc <service-name> -p '{"spec":{"sessionAffinity":"ClientIP"}}'
```

