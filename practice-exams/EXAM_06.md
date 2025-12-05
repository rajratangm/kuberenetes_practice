# CKA Practice Exam 6 - Storage & StatefulSets

**Duration**: 2 hours  
**Total Tasks**: 16  
**Passing Score**: 66% (74% for CKA)

---

## Task 1: Basic PersistentVolume (10 minutes)

Create PV `pv-basic` with:
- 10Gi storage
- ReadWriteOnce access mode
- Storage class `manual`
- Host path `/mnt/data`
- Reclaim policy `Retain`

---

## Task 2: PersistentVolumeClaim (10 minutes)

Create PVC `data-pvc` that:
- Requests 5Gi from storage class `manual`
- Uses ReadWriteOnce access mode
- Binds to the PV from Task 1
- Verify binding

---

## Task 3: Pod with PVC (10 minutes)

Create pod `app-pod` that:
- Uses the PVC from Task 2
- Mounts at `/data`
- Writes test file
- Verify persistence

---

## Task 4: StorageClass Creation (10 minutes)

Create StorageClass `fast-ssd` with:
- Provisioner `kubernetes.io/no-provisioner`
- Volume binding mode `WaitForFirstConsumer`
- Reclaim policy `Delete`
- Set as default StorageClass

---

## Task 5: Dynamic Provisioning (12 minutes)

1. Create StorageClass with dynamic provisioner
2. Create PVC using this StorageClass
3. Create pod that uses the PVC
4. Verify dynamic provisioning worked

---

## Task 6: StatefulSet Basic (12 minutes)

Create StatefulSet `web-sts` with:
- 3 replicas
- Headless service `web-headless`
- Image `nginx:1.21`
- Verify ordered creation
- Check stable network identities

---

## Task 7: StatefulSet with Storage (15 minutes)

Create StatefulSet `db-sts` with:
- 3 replicas
- Image `mysql:8.0` with env vars
- Headless service
- Volume claim template: 10Gi, ReadWriteOnce
- Verify each pod has its own PVC

---

## Task 8: StatefulSet Scaling (10 minutes)

1. Scale StatefulSet from 3 to 5 replicas
2. Verify new pods get their own PVCs
3. Scale down to 2 replicas
4. Verify PVCs are retained (not deleted)

---

## Task 9: StatefulSet Update Strategy (12 minutes)

Create StatefulSet with:
- 5 replicas
- Update strategy: RollingUpdate with partition 3
- Update image
- Verify only pods 0,1,2 update
- Update remaining pods

---

## Task 10: Multiple Volume Types (12 minutes)

Create pod with multiple volume types:
- emptyDir volume
- ConfigMap volume
- Secret volume
- PVC volume
- Verify all mount correctly

---

## Task 11: Volume Expansion (10 minutes)

1. Create StorageClass with `allowVolumeExpansion: true`
2. Create PVC
3. Expand PVC from 5Gi to 10Gi
4. Verify expansion

---

## Task 12: Access Modes (10 minutes)

Create and understand:
- PV with ReadWriteOnce
- PV with ReadWriteMany
- PV with ReadOnlyMany
- Create appropriate PVCs for each

---

## Task 13: Storage Reclaim Policies (10 minutes)

Create PVs with different reclaim policies:
- `Retain` - verify PVC deletion doesn't delete PV
- `Delete` - verify PVC deletion deletes PV
- `Recycle` - if supported

---

## Task 14: StatefulSet Pod Management (12 minutes)

1. Delete pod from StatefulSet
2. Verify it's recreated with same name
3. Verify it reattaches to same PVC
4. Check data persistence

---

## Task 15: Volume Snapshots (12 minutes)

Document procedure for:
1. Creating VolumeSnapshotClass
2. Creating VolumeSnapshot
3. Restoring from snapshot
4. Using restored volume

(Note: May not be available in all clusters)

---

## Task 16: Storage Troubleshooting (12 minutes)

Fix storage issues:
1. PVC in Pending state
2. PV not binding to PVC
3. Pod can't mount volume
4. Data not persisting

---

## Scoring

- **Tasks 1-5**: 6 points each (30 points)
- **Tasks 6-10**: 7 points each (35 points)
- **Tasks 11-15**: 6 points each (30 points)
- **Task 16**: 5 points

**Total**: 100 points  
**Passing**: 66 points (74 for CKA)

---

**Good luck!** 🚀

