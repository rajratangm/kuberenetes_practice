# Scenario Solutions - Quick Reference YAML Files

This directory contains quick reference YAML solution files for common scenario-based questions.

## Files

### Basic Scenarios
- `01-pod-pending-solution.yaml` - Fixed pod after diagnosing pending issue
- `02-service-fix-solution.yaml` - Service with correct selector
- `03-deployment-scaling-solution.yaml` - Deployment scaling configuration
- `04-configmap-pod-solution.yaml` - Pod with ConfigMap volume mount
- `05-secret-pod-solution.yaml` - Pod with Secret as environment variables

### Intermediate Scenarios
- `06-rolling-update-solution.yaml` - Deployment rolling update fix
- `07-pvc-binding-solution.yaml` - PV and PVC binding solution
- `08-networkpolicy-solution.yaml` - NetworkPolicy allowing specific traffic
- `09-rbac-solution.yaml` - Role and RoleBinding for permissions
- `10-ingress-solution.yaml` - Correctly configured Ingress
- `11-statefulset-pod-solution.yaml` - StatefulSet pod startup fix
- `12-job-completion-solution.yaml` - Job completion configuration

### Advanced Scenarios
- `13-multi-namespace-solution.yaml` - Cross-namespace service discovery
- `14-resource-exhaustion-solution.yaml` - Resource quota and optimization
- `15-etcd-backup-solution.yaml` - etcd backup procedure (commands)
- `16-node-maintenance-solution.yaml` - Node maintenance procedure (commands)
- `17-deployment-rollback-solution.yaml` - Deployment rollback (commands)
- `18-pod-security-solution.yaml` - Pod Security Standards compliance
- `19-networkpolicy-mesh-solution.yaml` - Service mesh-like NetworkPolicies
- `20-multi-container-solution.yaml` - Multi-container pod coordination
- `21-resource-quota-solution.yaml` - ResourceQuota configuration
- `22-cronjob-solution.yaml` - CronJob configuration
- `23-daemonset-solution.yaml` - DaemonSet on all nodes
- `24-statefulset-scaling-solution.yaml` - StatefulSet scaling with storage
- `25-pdb-update-solution.yaml` - Pod Disruption Budget and updates

## Usage

These are quick reference files. For complete step-by-step solutions with commands and explanations, see:
- `SCENARIO_SOLUTIONS_DETAILED.md` - Complete solutions
- `SCENARIO_SOLUTIONS.md` - Quick troubleshooting patterns

## How to Use

1. Read the scenario in `SCENARIO_BASED_QUESTIONS.md`
2. Try to solve it yourself
3. Check the corresponding solution file here
4. Review detailed solution in `SCENARIO_SOLUTIONS_DETAILED.md`
5. Practice again until you can do it quickly

## Note

These are example solutions. In the actual exam, you may need to adapt based on the specific requirements and current cluster state.

