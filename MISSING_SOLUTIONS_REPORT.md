# Missing Solutions Report

## Summary
This document identifies questions that reference solution files that don't exist.

## Folder Analysis

### ✅ 00-basic-to-advanced/01-basics
- **Status**: ✅ Complete
- **Questions**: 18
- **Solutions**: Inline in README (no separate YAML files needed)
- **Missing**: None

### ✅ 00-basic-to-advanced/02-intermediate
- **Status**: ✅ Complete
- **Questions**: 18
- **Solution Files Referenced**: 12
- **Solution Files Found**: 12
- **Missing**: None ✅

### ⚠️ 00-basic-to-advanced/03-advanced
- **Status**: ⚠️ Mostly Complete (2 missing)
- **Questions**: 35
- **Solution Files Referenced**: 30
- **Solution Files Found**: 28
- **Missing**: 
  1. `volumesnapshot.yaml` (optional - may not be supported)
  2. `troubleshooting-multi.yaml` (referenced but not found)

### ❌ 05-troubleshooting
- **Status**: ❌ CRITICAL - Missing 40+ solution files
- **Questions**: 50+
- **Solution Files**: Only 4
- **Missing Solution Files Needed**:
  1. `pod-imagepullbackoff-issue.yaml` (problematic pod)
  2. `pod-imagepullbackoff-fixed.yaml` (fixed version)
  3. `pod-oomkilled-issue.yaml`
  4. `pod-oomkilled-fixed.yaml`
  5. `pod-startup-probe-failure.yaml`
  6. `pod-startup-probe-fixed.yaml`
  7. `pod-init-container-failure.yaml`
  8. `pod-init-container-fixed.yaml`
  9. `service-port-mismatch-issue.yaml`
  10. `service-port-mismatch-fixed.yaml`
  11. `service-dns-issue.yaml`
  12. `service-dns-fixed.yaml`
  13. `service-session-affinity-issue.yaml`
  14. `service-session-affinity-fixed.yaml`
  15. `deployment-replica-mismatch-issue.yaml`
  16. `deployment-replica-mismatch-fixed.yaml`
  17. `deployment-rollout-stuck-issue.yaml`
  18. `deployment-rollout-stuck-fixed.yaml`
  19. `deployment-history-issue.yaml`
  20. `node-disk-pressure-issue.yaml`
  21. `node-memory-pressure-issue.yaml`
  22. `node-network-issue.yaml`
  23. `node-kubelet-issue.yaml`
  24. `networkpolicy-blocking-issue.yaml`
  25. `networkpolicy-blocking-fixed.yaml`
  26. `ingress-not-routing-issue.yaml`
  27. `ingress-not-routing-fixed.yaml`
  28. `pvc-not-binding-issue.yaml`
  29. `pvc-not-binding-fixed.yaml`
  30. `pod-cannot-mount-volume-issue.yaml`
  31. `pod-cannot-mount-volume-fixed.yaml`
  32. `volume-readonly-issue.yaml`
  33. `volume-readonly-fixed.yaml`
  34. `rbac-permission-denied-issue.yaml`
  35. `rbac-permission-denied-fixed.yaml`
  36. `serviceaccount-token-missing-issue.yaml`
  37. `serviceaccount-token-missing-fixed.yaml`
  38. `resourcequota-exceeded-issue.yaml`
  39. `resourcequota-exceeded-fixed.yaml`

### ✅ All Other Folders (01-08)
- **Status**: ✅ Complete
- All questions have corresponding solution files

## Priority Actions

### 🔴 HIGH PRIORITY
1. **Create missing solution files for 05-troubleshooting/** (40+ files)
   - This is 30% of the CKA exam!
   - Include both "problematic" and "fixed" versions
   - Add diagnostic command examples

### 🟡 MEDIUM PRIORITY
2. **Create missing files for 00-basic-to-advanced/03-advanced/**
   - `troubleshooting-multi.yaml`
   - `volumesnapshot.yaml` (if supported)

## Next Steps
1. Create all missing troubleshooting solution files
2. Add diagnostic commands to troubleshooting README
3. Verify all solution files match questions correctly

