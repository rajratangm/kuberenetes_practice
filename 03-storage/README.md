# Scenario 3: Storage

## Scenario 3.1: PersistentVolumes and PersistentVolumeClaims

### Question 3.1.1: Static Provisioning
Create a PersistentVolume (PV) and PersistentVolumeClaim (PVC):
- PV name: `pv-manual`
- Storage: 5Gi
- Access mode: ReadWriteOnce
- Storage class: manual
- Host path: `/mnt/data`
- Reclaim policy: Retain

- PVC name: `pvc-manual`
- Request: 3Gi
- Access mode: ReadWriteOnce
- Storage class: manual

**Solution Steps:**
1. Check `solutions/pv-manual.yaml` and `solutions/pvc-manual.yaml`
2. Apply PV first: `kubectl apply -f solutions/pv-manual.yaml`
3. Apply PVC: `kubectl apply -f solutions/pvc-manual.yaml`
4. Verify binding: `kubectl get pv` and `kubectl get pvc`
5. Check status: `kubectl describe pvc pvc-manual`

### Question 3.1.2: Use PVC in Pod
Create a Pod that uses the PVC:
- Pod name: `app-with-storage`
- Image: `nginx:1.21`
- Mount PVC `pvc-manual` at `/var/www/html`

**Solution Steps:**
1. Check `solutions/pod-with-pvc.yaml`
2. Apply: `kubectl apply -f solutions/pod-with-pvc.yaml`
3. Verify: `kubectl exec app-with-storage -- ls -la /var/www/html`
4. Write test file: `kubectl exec app-with-storage -- sh -c "echo 'test' > /var/www/html/test.txt"`
5. Delete pod and recreate to verify persistence

### Question 3.1.3: Multiple Access Modes
Create PVs with different access modes:
- PV1: ReadWriteOnce (RWO)
- PV2: ReadWriteMany (RWX)
- PV3: ReadOnlyMany (ROX)

**Solution Steps:**
1. Check `solutions/pv-rwo.yaml`, `solutions/pv-rwx.yaml`, `solutions/pv-rox.yaml`
2. Understand when each access mode is used

## Scenario 3.2: StorageClasses

### Question 3.2.1: Create StorageClass
Create a StorageClass named `fast-ssd` that:
- Provisioner: `kubernetes.io/no-provisioner` (for local storage)
- Volume binding mode: WaitForFirstConsumer
- Reclaim policy: Delete

**Solution Steps:**
1. Check `solutions/storageclass-fast-ssd.yaml`
2. Apply: `kubectl apply -f solutions/storageclass-fast-ssd.yaml`
3. Verify: `kubectl get storageclass`

### Question 3.2.2: Dynamic Provisioning with PVC
Create a PVC that uses dynamic provisioning:
- PVC name: `pvc-dynamic`
- Storage class: `fast-ssd`
- Request: 10Gi
- Access mode: ReadWriteOnce

**Solution Steps:**
1. Check `solutions/pvc-dynamic.yaml`
2. Apply and verify automatic PV creation
3. Check: `kubectl get pv` (should see auto-created PV)

### Question 3.2.3: Default StorageClass
Set `fast-ssd` as the default StorageClass.

**Solution Steps:**
1. Edit StorageClass: `kubectl patch storageclass fast-ssd -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`
2. Verify: `kubectl get storageclass` (should show default)

## Scenario 3.3: Volume Types

### Question 3.3.1: emptyDir Volume
Create a Pod with emptyDir volume:
- Pod name: `pod-emptydir`
- Two containers sharing emptyDir at `/shared`
- Container 1 writes data, Container 2 reads it

**Solution Steps:**
1. Check `solutions/pod-emptydir.yaml`
2. Apply and test

### Question 3.3.2: ConfigMap Volume
Create a Pod with ConfigMap mounted as volume:
- Pod name: `pod-configmap-volume`
- ConfigMap: `app-config` (create if needed)
- Mount at `/etc/config`

**Solution Steps:**
1. Check `solutions/pod-configmap-volume.yaml`
2. Apply and verify: `kubectl exec pod-configmap-volume -- ls /etc/config`

### Question 3.3.3: Secret Volume
Create a Pod with Secret mounted as volume:
- Pod name: `pod-secret-volume`
- Secret: `db-secret` with username and password
- Mount at `/etc/secrets`

**Solution Steps:**
1. Create secret first (see `solutions/secret-db.yaml`)
2. Check `solutions/pod-secret-volume.yaml`
3. Apply and verify

## Scenario 3.4: StatefulSets with Storage

### Question 3.4.1: StatefulSet with PVC
Create a StatefulSet that:
- Name: `web-statefulset`
- Replicas: 3
- Image: `nginx:1.21`
- Each pod gets its own PVC (volumeClaimTemplates)
- Storage: 1Gi per pod
- Storage class: `fast-ssd`

**Solution Steps:**
1. Check `solutions/statefulset-with-storage.yaml`
2. Apply: `kubectl apply -f solutions/statefulset-with-storage.yaml`
3. Verify: `kubectl get statefulset`, `kubectl get pvc`
4. Check pod naming: `kubectl get pods -l app=web`
5. Each pod should have its own PVC: `web-statefulset-0`, `web-statefulset-1`, etc.

### Question 3.1.4: PV Binding and Reclaim Policies
Create multiple PVs with different reclaim policies:
- PV1: Reclaim policy Retain
- PV2: Reclaim policy Delete
- PV3: Reclaim policy Recycle (deprecated)
- Understand when each is used

**Solution Steps:**
1. Create PVs with different reclaim policies
2. Bind PVCs and delete them
3. Observe PV behavior after PVC deletion

### Question 3.1.5: PVC with StorageClass
Create a PVC that:
- Name: `pvc-sc`
- Uses StorageClass `fast-ssd`
- Request: 20Gi
- Access mode: ReadWriteOnce
- Verify PV is created automatically

**Solution Steps:**
1. Ensure StorageClass exists
2. Create PVC
3. Verify PV auto-creation: `kubectl get pv`
4. Check binding: `kubectl get pvc pvc-sc`

### Question 3.1.6: PVC Expansion
Create a PVC and expand it:
- Initial size: 5Gi
- Expand to: 10Gi
- Verify expansion works

**Solution Steps:**
1. Create PVC with 5Gi
2. Edit: `kubectl edit pvc <pvc-name>` (change to 10Gi)
3. Verify: `kubectl get pvc`
4. Note: Requires StorageClass with `allowVolumeExpansion: true`

### Question 3.1.7: PV with Node Affinity
Create a PV with node affinity:
- PV name: `pv-node-affinity`
- Storage: 10Gi
- Node affinity to specific zone
- Used for local storage

**Solution Steps:**
1. Check `solutions/pv-node-affinity.yaml`
2. Apply and verify node affinity
3. Understand use case for local storage

### Question 3.1.8: Multiple PVCs from Same PV
Create a PV that can be shared:
- PV: 20Gi, ReadWriteMany
- PVC1: 5Gi
- PVC2: 5Gi
- Both bind to same PV

**Solution Steps:**
1. Create PV with RWX access mode
2. Create multiple PVCs
3. Verify they can bind (if supported by storage)

## Scenario 3.2: StorageClasses

### Question 3.2.4: StorageClass with Parameters
Create a StorageClass with parameters:
- Name: `parameterized-sc`
- Provisioner: `kubernetes.io/aws-ebs`
- Parameters: type=gp3, iops=3000
- Volume binding: WaitForFirstConsumer

**Solution Steps:**
1. Check `solutions/storageclass-parameters.yaml`
2. Apply and verify parameters
3. Note: Actual provisioner depends on environment

### Question 3.2.5: Multiple StorageClasses
Create multiple StorageClasses for different use cases:
- `fast-ssd`: High performance
- `standard-hdd`: Cost effective
- `backup-storage`: For backups

**Solution Steps:**
1. Create all three StorageClasses
2. Understand when to use each
3. Set one as default

### Question 3.2.6: StorageClass Allow Expansion
Create a StorageClass that allows volume expansion:
- Name: `expandable-sc`
- `allowVolumeExpansion: true`
- Test PVC expansion

**Solution Steps:**
1. Create StorageClass with expansion enabled
2. Create PVC using this StorageClass
3. Expand PVC and verify

## Scenario 3.3: Volume Types

### Question 3.3.4: Projected Volume
Create a Pod with projected volume containing:
- ConfigMap
- Secret
- Downward API
- All mounted in one volume

**Solution Steps:**
1. Check `solutions/pod-projected-volume.yaml`
2. Apply and verify all sources are mounted
3. Test: `kubectl exec <pod> -- ls /projected-volume`

### Question 3.3.5: Downward API Volume
Create a Pod that exposes pod metadata via Downward API:
- Pod name as file
- Namespace as file
- Labels as files
- Annotations as files

**Solution Steps:**
1. Check `solutions/pod-downward-api.yaml`
2. Apply and verify metadata files
3. Understand use cases

### Question 3.3.6: Volume with SubPath
Create a Pod that mounts specific keys from ConfigMap/Secret:
- ConfigMap with multiple keys
- Mount only specific key using subPath
- Verify only that key is mounted

**Solution Steps:**
1. Create ConfigMap with multiple keys
2. Mount using subPath
3. Verify only selected key is present

### Question 3.3.7: Volume with Mount Propagation
Create a Pod with volume mount propagation:
- Mount propagation: Bidirectional
- Share volume between containers
- Test propagation behavior

**Solution Steps:**
1. Check `solutions/pod-mount-propagation.yaml`
2. Apply and test mount propagation
3. Understand use cases

## Scenario 3.4: StatefulSets with Storage

### Question 3.4.2: StatefulSet Volume Management
Create a StatefulSet and understand volume lifecycle:
- Create StatefulSet with 3 replicas
- Each pod gets its own PVC
- Delete a pod and observe PVC persistence
- Scale down and scale up

**Solution Steps:**
1. Create StatefulSet with volumeClaimTemplates
2. Verify each pod has its own PVC
3. Delete pod: `kubectl delete pod web-statefulset-0`
4. Verify new pod uses same PVC

### Question 3.4.3: StatefulSet Ordered Pod Management
Understand StatefulSet pod management:
- Create StatefulSet with 5 replicas
- Observe ordered creation (0, 1, 2, 3, 4)
- Observe ordered deletion (4, 3, 2, 1, 0)
- Test scaling

**Solution Steps:**
1. Create StatefulSet
2. Watch: `kubectl get pods -w -l app=web`
3. Scale: `kubectl scale statefulset web-statefulset --replicas=3`
4. Observe ordered scaling

### Question 3.4.4: StatefulSet with Multiple Volume Claims
Create a StatefulSet with multiple volumeClaimTemplates:
- Volume 1: Data (10Gi)
- Volume 2: Logs (5Gi)
- Each pod gets both volumes

**Solution Steps:**
1. Check `solutions/statefulset-multi-volume.yaml`
2. Apply and verify both volumes per pod
3. Check: `kubectl get pvc`

## Scenario 3.5: Volume Snapshots (Conceptual)

### Question 3.5.1: Understand Volume Snapshots
Understand VolumeSnapshot concepts:
- What are volume snapshots
- When to use them
- How they relate to PVCs

**Solution Steps:**
1. Research VolumeSnapshot API
2. Understand backup/restore use cases
3. Note: May not be in all CKA exams but good to know

## Practice Commands

```bash
# Get PVs and PVCs
kubectl get pv
kubectl get pvc
kubectl get pvc -A
kubectl get pv,pvc

# Describe storage resources
kubectl describe pv <pv-name>
kubectl describe pvc <pvc-name>
kubectl describe storageclass <sc-name>

# Get StorageClasses
kubectl get storageclass
kubectl get sc

# Check volume mounts in pod
kubectl describe pod <pod-name> | grep -A 5 Mounts
kubectl get pod <pod-name> -o jsonpath='{.spec.volumes}'

# Delete PVC (and bound PV if reclaim policy allows)
kubectl delete pvc <pvc-name>
kubectl delete pv <pv-name>

# Check volume status
kubectl get pv -o yaml
kubectl get pvc -o yaml

# Expand PVC
kubectl patch pvc <pvc-name> -p '{"spec":{"resources":{"requests":{"storage":"10Gi"}}}}'

# Check volume in use
kubectl get pv -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.claimRef.name}{"\n"}{end}'

# StatefulSet volumes
kubectl get pvc -l app=web
kubectl describe statefulset <sts-name> | grep -A 10 "Volume Claims"
```

