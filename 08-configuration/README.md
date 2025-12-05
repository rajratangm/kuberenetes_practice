# Scenario 8: Configuration

## Scenario 8.1: ConfigMaps

### Question 8.1.1: Create ConfigMap from Literal
Create a ConfigMap named `app-config` with:
- database_host: localhost
- database_port: 5432
- log_level: info

**Solution Steps:**
1. Check `solutions/configmap-literal.yaml`
2. Apply: `kubectl apply -f solutions/configmap-literal.yaml`
3. Verify: `kubectl get configmap app-config -o yaml`
4. Alternative: `kubectl create configmap app-config --from-literal=database_host=localhost --from-literal=database_port=5432 --from-literal=log_level=info`

### Question 8.1.2: Create ConfigMap from File
Create a ConfigMap from a file:
- Name: `app-config-file`
- File: `config.properties` with content:
  ```
  server.port=8080
  server.host=0.0.0.0
  ```

**Solution Steps:**
1. Create file first (see `solutions/config.properties`)
2. Check `solutions/configmap-from-file.yaml` or use:
   `kubectl create configmap app-config-file --from-file=config.properties`

### Question 8.1.3: Use ConfigMap in Pod
Use ConfigMap in a Pod as:
- Environment variables
- Volume mount

**Solution Steps:**
1. Check `solutions/pod-configmap-env.yaml` for env vars
2. Check `solutions/pod-configmap-volume.yaml` for volume mount
3. Apply and verify

### Question 8.1.4: ConfigMap with envFrom
Create a Pod that uses ConfigMap with envFrom:
- Load all ConfigMap keys as environment variables
- Use envFrom instead of individual env entries

**Solution Steps:**
1. Check `solutions/pod-configmap-envfrom.yaml`
2. Apply and verify all keys are loaded
3. Test: `kubectl exec <pod> -- env | grep -E "database|log"`

### Question 8.1.5: ConfigMap with Optional Keys
Create a Pod that uses ConfigMap with optional keys:
- Some keys may not exist
- Pod should start even if key missing
- Use optional: true

**Solution Steps:**
1. Create ConfigMap with some keys
2. Create pod referencing missing key with optional: true
3. Verify pod starts successfully

### Question 8.1.6: ConfigMap Hot Reload
Understand ConfigMap updates:
- Update ConfigMap
- Pods using volume mount need restart
- Pods using env vars need restart
- Test update behavior

**Solution Steps:**
1. Create ConfigMap and pod using volume mount
2. Update ConfigMap: `kubectl create configmap app-config --from-literal=key=newvalue --dry-run=client -o yaml | kubectl apply -f -`
3. Check if pod picks up changes (may need restart)
4. Understand sync period limitations

## Scenario 8.2: Secrets

### Question 8.2.1: Create Secret
Create a Secret named `db-credentials` with:
- username: admin
- password: secretpass123

**Solution Steps:**
1. Check `solutions/secret-db-credentials.yaml`
2. Apply: `kubectl apply -f solutions/secret-db-credentials.yaml`
3. Verify: `kubectl get secret db-credentials -o yaml`
4. Decode: `kubectl get secret db-credentials -o jsonpath='{.data.password}' | base64 -d`

### Question 8.2.2: Create Secret from File
Create a Secret from files:
- Name: `tls-secret`
- Files: `tls.crt` and `tls.key`

**Solution Steps:**
1. Generate certs (for testing): `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=example.com"`
2. Create secret: `kubectl create secret tls tls-secret --cert=tls.crt --key=tls.key`
3. Verify: `kubectl get secret tls-secret`

### Question 8.2.3: Secret with envFrom
Create a Pod that uses Secret with envFrom:
- Load all Secret keys as environment variables
- Use envFrom for multiple keys

**Solution Steps:**
1. Check `solutions/pod-secret-envfrom.yaml`
2. Apply and verify all keys are loaded
3. Test: `kubectl exec <pod> -- env | grep -E "USERNAME|PASSWORD"`

### Question 8.2.4: Secret Optional Keys
Create a Pod that uses Secret with optional keys:
- Some keys may not exist
- Pod should start even if key missing

**Solution Steps:**
1. Create Secret with some keys
2. Create pod referencing missing key with optional: true
3. Verify pod starts successfully

## Scenario 8.3: Resource Quotas and Limits

### Question 8.3.1: ResourceQuota (covered in Cluster Maintenance)

### Question 8.3.2: LimitRange (covered in Cluster Maintenance)

## Scenario 8.4: Pod Disruption Budgets

### Question 8.4.1: Create PDB
Create a PodDisruptionBudget that:
- Name: `web-pdb`
- Min available: 2 pods
- Selector: `app=web`

**Solution Steps:**
1. Check `solutions/pdb-web.yaml`
2. Apply: `kubectl apply -f solutions/pdb-web.yaml`
3. Verify: `kubectl get pdb web-pdb`
4. Test: Try to drain a node and see PDB in action

### Question 8.4.2: PDB with Max Unavailable
Create a PDB with max unavailable: 1

**Solution Steps:**
1. Check `solutions/pdb-max-unavailable.yaml`
2. Apply and verify

### Question 8.4.3: PDB with Selector
Create a PDB with complex selector:
- Match labels: `app=web`, `env=production`
- Min available: 3
- Test with multiple deployments

**Solution Steps:**
1. Check `solutions/pdb-selector.yaml`
2. Apply and verify selector matches correct pods
3. Test by draining nodes

### Question 8.4.4: PDB for StatefulSet
Create a PDB for StatefulSet:
- StatefulSet with 5 replicas
- PDB with min available: 3
- Understand StatefulSet + PDB interaction

**Solution Steps:**
1. Create StatefulSet
2. Create PDB for StatefulSet pods
3. Test disruption behavior

## Scenario 8.5: Affinity and Anti-Affinity

### Question 8.5.1: Pod Affinity
Create a Deployment with pod affinity:
- Pods should be scheduled on nodes that have pods with label `app=cache`
- Preferred (soft) affinity

**Solution Steps:**
1. Check `solutions/deployment-pod-affinity.yaml`
2. Apply and verify pod placement

### Question 8.5.2: Pod Anti-Affinity
Create a Deployment with pod anti-affinity:
- Pods should not be scheduled on same node (required)
- Label selector: `app=web`

**Solution Steps:**
1. Check `solutions/deployment-pod-antiaffinity.yaml`
2. Apply and verify pods are on different nodes

### Question 8.5.3: Node Affinity
Create a Deployment with node affinity:
- Required: node with label `node-type=compute`
- Preferred: node with label `zone=us-east-1a`

**Solution Steps:**
1. Check `solutions/deployment-node-affinity.yaml`
2. Apply and verify

### Question 8.5.4: Node Affinity with Operators
Create a Deployment with node affinity using different operators:
- In: node-type in (compute, gpu)
- NotIn: zone not in (us-west-1)
- Exists: label exists
- DoesNotExist: label doesn't exist
- Gt/Lt: for numeric values

**Solution Steps:**
1. Check `solutions/deployment-node-affinity-operators.yaml`
2. Apply and verify operator behavior
3. Test each operator type

### Question 8.5.5: Pod Affinity with Topology
Create a Deployment with pod affinity using topology keys:
- Affinity to pods with label `app=cache`
- Topology key: `kubernetes.io/hostname` (same node)
- Topology key: `topology.kubernetes.io/zone` (same zone)

**Solution Steps:**
1. Check `solutions/deployment-pod-affinity-topology.yaml`
2. Apply and verify topology-based placement
3. Understand different topology keys

## Scenario 8.6: Taints and Tolerations

### Question 8.6.1: Pod with Tolerations
Create a Pod that can tolerate a taint:
- Taint: `dedicated=special:NoSchedule`
- Pod should have matching toleration

**Solution Steps:**
1. First taint a node: `kubectl taint nodes <node-name> dedicated=special:NoSchedule`
2. Check `solutions/pod-with-toleration.yaml`
3. Apply pod and verify it schedules on tainted node

### Question 8.6.2: Toleration with Effect
Create Pods with tolerations for different taint effects:
- NoSchedule: Pod won't be scheduled
- PreferNoSchedule: Pod prefers not to schedule
- NoExecute: Pod will be evicted if already running

**Solution Steps:**
1. Create taints with different effects
2. Create pods with matching tolerations
3. Observe scheduling behavior

### Question 8.6.3: Toleration with TolerationSeconds
Create a Pod with toleration that has TolerationSeconds:
- Tolerate NoExecute taint
- TolerationSeconds: 60
- Pod will be evicted after 60 seconds

**Solution Steps:**
1. Check `solutions/pod-toleration-seconds.yaml`
2. Apply and verify eviction timing
3. Understand use cases

## Scenario 8.7: Advanced Configuration Patterns

### Question 8.7.1: ConfigMap with Binary Data
Create ConfigMap with binary data:
- Name: `binary-config`
- Binary data key: `binary-file`
- Use in pod

**Solution Steps:**
1. Create ConfigMap with binaryData
2. Mount in pod
3. Verify binary data integrity

### Question 8.7.2: Immutable ConfigMap
Create immutable ConfigMap:
- Name: `immutable-config`
- Immutable: true
- Cannot be updated after creation

**Solution Steps:**
1. Check `solutions/configmap-immutable.yaml`
2. Apply and verify
3. Attempt update (should fail)
4. Delete and recreate to update

### Question 8.7.3: Immutable Secret
Create immutable Secret:
- Name: `immutable-secret`
- Immutable: true
- Cannot be updated after creation

**Solution Steps:**
1. Create immutable secret
2. Verify immutability
3. Understand use cases

### Question 8.7.4: ConfigMap with Multiple Files
Create ConfigMap from directory:
- Name: `multi-file-config`
- Directory with multiple config files
- All files in one ConfigMap

**Solution Steps:**
1. Create directory with files
2. Create ConfigMap: `kubectl create configmap multi-file-config --from-file=<directory>`
3. Verify all files included
4. Mount in pod

### Question 8.7.5: Secret with Multiple Keys
Create Secret with many keys:
- Name: `multi-key-secret`
- 10+ different keys
- Use specific keys in different pods

**Solution Steps:**
1. Create secret with multiple keys
2. Use different keys in different pods
3. Verify key isolation

## Scenario 8.8: Advanced Affinity Scenarios

### Question 8.8.1: Combined Affinity Rules
Create Deployment with combined affinity:
- Pod affinity AND pod anti-affinity
- Node affinity AND pod affinity
- Complex scheduling requirements

**Solution Steps:**
1. Check `solutions/deployment-combined-affinity.yaml`
2. Apply and verify complex scheduling
3. Understand rule evaluation

### Question 8.8.2: Affinity with Weight
Create Deployment with weighted affinity:
- Preferred affinity with different weights
- Higher weight = stronger preference
- Test scheduling with weights

**Solution Steps:**
1. Create deployment with weighted affinity
2. Verify weight influence on scheduling
3. Adjust weights and observe changes

### Question 8.8.3: Anti-Affinity for High Availability
Create Deployment ensuring HA:
- Pod anti-affinity across zones
- Pod anti-affinity across nodes
- Ensure no single point of failure

**Solution Steps:**
1. Create deployment with zone anti-affinity
2. Verify pods spread across zones
3. Test node failure scenario

## Scenario 8.9: Resource Management Integration

### Question 8.9.1: PDB with ResourceQuota
Create system with both PDB and ResourceQuota:
- ResourceQuota limits resources
- PDB ensures availability
- Both work together

**Solution Steps:**
1. Create ResourceQuota
2. Create PDB
3. Verify both enforce correctly
4. Test interaction

### Question 8.9.2: LimitRange with ResourceQuota
Create namespace with both:
- LimitRange sets defaults
- ResourceQuota sets maximums
- Verify interaction

**Solution Steps:**
1. Create LimitRange
2. Create ResourceQuota
3. Create pods and verify both apply
4. Test edge cases

### Question 8.9.3: Priority Classes
Create and use PriorityClasses:
- High priority class
- Low priority class
- Assign to pods
- Understand eviction behavior

**Solution Steps:**
1. Create PriorityClass: `kubectl create priorityclass high-priority --value=1000`
2. Assign to pod
3. Test eviction behavior
4. Understand preemption

## Practice Commands

```bash
# ConfigMaps
kubectl create configmap <name> --from-literal=<key>=<value>
kubectl create configmap <name> --from-file=<file>
kubectl get configmap <name> -o yaml
kubectl describe configmap <name>

# Secrets
kubectl create secret generic <name> --from-literal=<key>=<value>
kubectl create secret tls <name> --cert=<cert-file> --key=<key-file>
kubectl get secret <name> -o yaml
kubectl get secret <name> -o jsonpath='{.data.<key>}' | base64 -d

# PDB
kubectl get pdb
kubectl describe pdb <name>

# Affinity
kubectl get pods -o wide  # Check pod placement
kubectl describe pod <pod-name> | grep -A 10 Affinity
```

