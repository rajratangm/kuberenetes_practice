# Scenario 4: Security

## Scenario 4.1: ServiceAccounts

### Question 4.1.1: Create ServiceAccount
Create a ServiceAccount named `api-serviceaccount` in namespace `production`.

**Solution Steps:**
1. Check `solutions/serviceaccount-api.yaml`
2. Apply: `kubectl apply -f solutions/serviceaccount-api.yaml`
3. Verify: `kubectl get serviceaccount api-serviceaccount -n production`

### Question 4.1.2: Use ServiceAccount in Pod
Create a Pod that uses the ServiceAccount:
- Pod name: `pod-with-sa`
- Image: `busybox:1.35`
- ServiceAccount: `api-serviceaccount`

**Solution Steps:**
1. Check `solutions/pod-with-serviceaccount.yaml`
2. Apply and verify: `kubectl describe pod pod-with-sa`

### Question 4.1.3: ServiceAccount with Image Pull Secrets
Create a ServiceAccount with image pull secrets:
- Name: `private-registry-sa`
- Image pull secret: `registry-secret`
- Use in a pod that pulls from private registry

**Solution Steps:**
1. Create image pull secret: `kubectl create secret docker-registry registry-secret --docker-server=registry.example.com --docker-username=user --docker-password=pass`
2. Check `solutions/serviceaccount-imagepull.yaml`
3. Apply and verify

### Question 4.1.4: Default ServiceAccount
Understand default ServiceAccount behavior:
- Every namespace has default ServiceAccount
- Pods use default SA if not specified
- Verify default SA permissions

**Solution Steps:**
1. Check: `kubectl get serviceaccount default -n <namespace>`
2. Create pod without specifying SA
3. Verify it uses default SA

## Scenario 4.2: RBAC - Roles and RoleBindings

### Question 4.2.1: Create Role
Create a Role in namespace `development` that allows:
- Get, list, watch pods
- Create, update, delete pods
- Name: `pod-manager-role`

**Solution Steps:**
1. Check `solutions/role-pod-manager.yaml`
2. Apply: `kubectl apply -f solutions/role-pod-manager.yaml`
3. Verify: `kubectl get role pod-manager-role -n development`
4. Check details: `kubectl describe role pod-manager-role -n development`

### Question 4.2.2: Create RoleBinding
Create a RoleBinding that:
- Binds `pod-manager-role` to ServiceAccount `api-serviceaccount`
- In namespace `development`
- Name: `pod-manager-binding`

**Solution Steps:**
1. Check `solutions/rolebinding-pod-manager.yaml`
2. Apply and verify
3. Test permissions (use kubectl auth can-i)

### Question 4.2.3: ClusterRole and ClusterRoleBinding
Create a ClusterRole that allows:
- Get, list all namespaces
- Get, list nodes
- Name: `cluster-viewer`

Create a ClusterRoleBinding that:
- Binds `cluster-viewer` to ServiceAccount `api-serviceaccount` in namespace `production`
- Name: `cluster-viewer-binding`

**Solution Steps:**
1. Check `solutions/clusterrole-viewer.yaml` and `solutions/clusterrolebinding-viewer.yaml`
2. Apply both
3. Verify: `kubectl get clusterrole cluster-viewer`

### Question 4.2.4: Role with Resource Names
Create a Role that allows access to specific resources by name:
- Name: `specific-pod-role`
- Get, update specific pod: `web-pod`
- List all pods in namespace

**Solution Steps:**
1. Check `solutions/role-resource-names.yaml`
2. Apply and test permissions
3. Verify: `kubectl auth can-i get pod web-pod --as=system:serviceaccount:default:api-sa`

### Question 4.2.5: Role with Subresources
Create a Role that allows:
- Get, list pods
- Get, list pod/logs (subresource)
- Exec into pods

**Solution Steps:**
1. Check `solutions/role-subresources.yaml`
2. Apply and verify subresource access

### Question 4.2.6: RoleBinding to User
Create a RoleBinding that binds role to a user:
- Role: `pod-reader`
- User: `developer`
- Namespace: `development`

**Solution Steps:**
1. Check `solutions/rolebinding-user.yaml`
2. Apply and verify
3. Test: `kubectl auth can-i get pods --as=developer -n development`

### Question 4.2.7: RoleBinding to Group
Create a RoleBinding that binds role to a group:
- Role: `namespace-admin`
- Group: `developers`
- Namespace: `development`

**Solution Steps:**
1. Check `solutions/rolebinding-group.yaml`
2. Apply and understand group-based access

### Question 4.2.8: ClusterRole for Namespace Resources
Create a ClusterRole that allows managing resources in all namespaces:
- Name: `namespace-admin`
- Verbs: get, list, create, update, delete
- Resources: pods, services, deployments

**Solution Steps:**
1. Check `solutions/clusterrole-namespace-admin.yaml`
2. Apply and verify cluster-wide permissions

### Question 4.2.9: ClusterRole for Cluster Resources
Create a ClusterRole that allows:
- Get, list nodes
- Get, list namespaces
- Get, list persistentvolumes

**Solution Steps:**
1. Check `solutions/clusterrole-cluster-resources.yaml`
2. Apply and verify cluster resource access

## Scenario 4.3: Secrets

### Question 4.3.1: Create Secret from Literal
Create a Secret named `app-secret` with:
- username: admin
- password: secret123
- api-key: abc123xyz

**Solution Steps:**
1. Check `solutions/secret-app.yaml`
2. Apply: `kubectl apply -f solutions/secret-app.yaml`
3. Verify: `kubectl get secret app-secret -o yaml`
4. Decode: `kubectl get secret app-secret -o jsonpath='{.data.password}' | base64 -d`

### Question 4.3.2: Use Secret in Pod as Environment Variables
Create a Pod that uses the secret as environment variables:
- Pod name: `pod-secret-env`
- Image: `busybox:1.35`
- Mount username and password as env vars

**Solution Steps:**
1. Check `solutions/pod-secret-env.yaml`
2. Apply and verify: `kubectl exec pod-secret-env -- env | grep -E "USERNAME|PASSWORD"`

### Question 4.3.3: Use Secret as Volume
Create a Pod that mounts the secret as a volume (see storage section for example).

### Question 4.3.4: Secret from File
Create a Secret from files:
- Name: `file-secret`
- Files: `username.txt`, `password.txt`
- Use in pod

**Solution Steps:**
1. Create files: `echo -n "admin" > username.txt && echo -n "pass123" > password.txt`
2. Create secret: `kubectl create secret generic file-secret --from-file=username.txt --from-file=password.txt`
3. Verify: `kubectl get secret file-secret -o yaml`

### Question 4.3.5: Secret with Different Types
Create secrets of different types:
- Opaque (generic)
- kubernetes.io/dockerconfigjson (for registry)
- kubernetes.io/tls (for TLS)
- kubernetes.io/basic-auth (for basic auth)

**Solution Steps:**
1. Create each type
2. Understand when to use each
3. Verify type: `kubectl get secret <name> -o jsonpath='{.type}'`

### Question 4.3.6: Secret Auto-Mount
Understand ServiceAccount secret auto-mounting:
- Create ServiceAccount
- Create pod using SA
- Verify token is auto-mounted

**Solution Steps:**
1. Create ServiceAccount
2. Create pod with SA
3. Check: `kubectl exec <pod> -- ls /var/run/secrets/kubernetes.io/serviceaccount`

### Question 4.3.7: Secret Rotation
Practice secret rotation:
- Create secret
- Update secret value
- Restart pods to pick up new secret

**Solution Steps:**
1. Create secret and pod using it
2. Update secret: `kubectl create secret generic app-secret --from-literal=key=newvalue --dry-run=client -o yaml | kubectl apply -f -`
3. Restart pods: `kubectl rollout restart deployment <name>`

## Scenario 4.4: Pod Security Policies (PSP) / Pod Security Standards

### Question 4.4.1: Pod Security Context
Create a Pod with security context:
- Run as non-root user (UID 1000)
- Run as group 2000
- Read-only root filesystem
- Drop all capabilities, add only NET_BIND_SERVICE

**Solution Steps:**
1. Check `solutions/pod-security-context.yaml`
2. Apply and verify: `kubectl describe pod pod-secure`

### Question 4.4.2: Pod Security Standards
Apply Pod Security Standards to a namespace:
- Namespace: `restricted`
- Level: restricted

**Solution Steps:**
1. Check `solutions/namespace-restricted.yaml`
2. Apply: `kubectl apply -f solutions/namespace-restricted.yaml`
3. Verify: `kubectl get namespace restricted -o yaml`

### Question 4.4.3: Pod Security Standards - Baseline
Apply baseline Pod Security Standard to namespace:
- Namespace: `baseline-ns`
- Level: baseline
- Test pod creation

**Solution Steps:**
1. Check `solutions/namespace-baseline.yaml`
2. Apply and test pod creation
3. Understand baseline restrictions

### Question 4.4.4: Pod Security Standards - Restricted
Apply restricted Pod Security Standard:
- Namespace: `restricted-ns`
- Level: restricted
- Try creating pod without security context

**Solution Steps:**
1. Apply restricted policy
2. Attempt to create pod without security context
3. Observe rejection
4. Create pod with proper security context

### Question 4.4.5: Pod Security Context - All Options
Create pod with comprehensive security context:
- Run as non-root
- Read-only root filesystem
- Drop all capabilities
- Add specific capabilities
- Set SELinux options
- Set seccomp profile

**Solution Steps:**
1. Check `solutions/pod-comprehensive-security.yaml`
2. Apply and verify all security settings
3. Test pod functionality

## Scenario 4.5: Network Policies (covered in Networking section)

## Scenario 4.6: Admission Controllers (Conceptual)

### Question 4.6.1: Understand Admission Controllers
Understand common admission controllers:
- ResourceQuota
- LimitRanger
- PodSecurityPolicy (deprecated, replaced by Pod Security Standards)
- AlwaysPullImages
- DefaultStorageClass

**Solution Steps:**
1. Research admission controllers
2. Understand their purpose
3. Note: Not directly configurable in CKA but good to know

## Practice Commands

```bash
# ServiceAccounts
kubectl get serviceaccount
kubectl get sa -A

# RBAC
kubectl get role
kubectl get rolebinding
kubectl get clusterrole
kubectl get clusterrolebinding

# Check permissions
kubectl auth can-i create pods --as=system:serviceaccount:default:api-serviceaccount
kubectl auth can-i --list --as=system:serviceaccount:default:api-serviceaccount

# Secrets
kubectl get secrets
kubectl get secret <secret-name> -o yaml
kubectl get secret <secret-name> -o jsonpath='{.data.password}' | base64 -d

# Security context
kubectl describe pod <pod-name> | grep -A 10 Security
kubectl get pod <pod-name> -o jsonpath='{.spec.securityContext}'
kubectl get pod <pod-name> -o jsonpath='{.spec.containers[0].securityContext}'

# Pod Security Standards
kubectl get namespace <name> -o jsonpath='{.metadata.labels}'
kubectl label namespace <name> pod-security.kubernetes.io/enforce=restricted

# Create RBAC resources
kubectl create role <name> --verb=get,list --resource=pods
kubectl create rolebinding <name> --role=<role-name> --serviceaccount=<namespace>:<sa-name>

# Secrets - additional commands
kubectl create secret docker-registry <name> --docker-server=<server> --docker-username=<user> --docker-password=<pass>
kubectl create secret tls <name> --cert=<cert-file> --key=<key-file>
```

