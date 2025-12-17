# CKA Practice Exams

This directory contains 10 complete CKA practice exams, each designed to simulate the real CKA exam experience.

## 📋 Exam Structure

Each exam includes:
- **Duration**: 2 hours (matching real CKA exam)
- **Tasks**: 15-20 tasks covering all CKA domains
- **Scoring**: 100 points total, 74 points to pass (66% passing score)
- **Format**: Performance-based (hands-on tasks in live cluster)
- **Solutions**: Complete solutions with commands and YAML

## 📚 Exam List

| Exam | Focus Area | Difficulty | Domain Coverage |
|------|------------|------------|-----------------|
| **EXAM_01.md** | Foundation & Core Concepts | ⭐⭐ | All domains (balanced) |
| **EXAM_02.md** | Intermediate Scenarios | ⭐⭐⭐ | All domains (balanced) |
| **EXAM_03.md** | Complex Multi-Tier Apps | ⭐⭐⭐ | All domains (balanced) |
| **EXAM_04.md** | Troubleshooting Heavy | ⭐⭐⭐⭐ | 40% Troubleshooting, 60% others |
| **EXAM_05.md** | Security & RBAC | ⭐⭐⭐ | Cluster Architecture focus |
| **EXAM_06.md** | Storage & StatefulSets | ⭐⭐⭐ | Storage + Workloads |
| **EXAM_07.md** | Networking & Ingress | ⭐⭐⭐ | Services & Networking focus |
| **EXAM_08.md** | Cluster Maintenance | ⭐⭐⭐⭐ | Cluster Architecture + Troubleshooting |
| **EXAM_09.md** | Advanced Patterns | ⭐⭐⭐⭐⭐ | All domains (advanced) |
| **EXAM_10.md** | Full Simulation | ⭐⭐⭐⭐ | Most realistic exam format |

## 🎯 Exam Domain Distribution

Each exam follows the official CKA curriculum weights:
- **Troubleshooting**: ~30% (largest domain)
- **Cluster Architecture**: ~25%
- **Services & Networking**: ~20%
- **Workloads & Scheduling**: ~15%
- **Storage**: ~10%

## 📖 How to Use

### Preparation Phase
1. ✅ Review `CKA_EXAM_CURRICULUM.md` in root directory
2. ✅ Practice domain-specific questions in `../domain-practice/`
3. ✅ Familiarize yourself with essential kubectl commands
4. ✅ Review cheat sheets in `../study-resources/`

### Taking the Exam (Realistic Simulation)
1. ⏱️ Set a timer for exactly 2 hours
2. 📝 Read all questions first to prioritize
3. 🎯 Work through tasks - don't spend more than 10-12 minutes per task
4. ⏭️ If stuck, move on and come back later
5. ✅ **ALWAYS verify your work** before moving on
6. 🔍 Use `kubectl explain` and official docs (allowed in real exam)

### After the Exam
1. 📊 Review solutions in `EXAM_*_SOLUTIONS.md`
2. 🔍 Identify weak domains
3. 📚 Practice those topics in `../domain-practice/`
4. 🔄 Retake the exam after improvement
5. 📈 Track your progress in `PRACTICE_EXAM_TRACKER.md`

## 📊 Exam Domains Covered

Each exam covers all official CKA domains with proper weight distribution:

| Domain | Weight | What's Tested |
|--------|--------|---------------|
| ✅ **Troubleshooting** | **30%** | Pod failures, Service issues, Cluster problems, Network debugging |
| ✅ **Cluster Architecture** | **25%** | RBAC, etcd backup/restore, Cluster upgrades, Node management |
| ✅ **Services & Networking** | **20%** | Services, Ingress, NetworkPolicies, DNS, Pod networking |
| ✅ **Workloads & Scheduling** | **15%** | Deployments, ConfigMaps, Secrets, Scaling, Scheduling |
| ✅ **Storage** | **10%** | PVs, PVCs, StorageClasses, Volume mounting |

## Scoring Guide

- **90-100%**: Excellent! Ready for real exam
- **80-89%**: Good, minor review needed
- **70-79%**: Needs more practice
- **Below 70%**: Focus on fundamentals

## 💡 Tips for Success

### ⏱️ Time Management Strategy
- **Quick tasks (5-8 min)**: Do immediately - easy points first
- **Medium tasks (10-12 min)**: Plan approach, then execute
- **Complex tasks (15-20 min)**: Break into steps, verify incrementally
- **Leave 10-15 min** at end for review and verification

### ✅ Verification Checklist (Do This After EVERY Task)
- [ ] Resource created/updated successfully
- [ ] `kubectl get` shows correct status
- [ ] Labels/selectors match correctly
- [ ] No errors in `kubectl describe`
- [ ] Events show no warnings/errors
- [ ] Connectivity works (if applicable)

### ⚠️ Common Mistakes to Avoid
- ❌ Wrong namespace (always check `-n` flag)
- ❌ Selector mismatches (labels don't match)
- ❌ Missing required fields in YAML
- ❌ Typos in resource names
- ❌ Forgetting to verify work
- ❌ Spending too long on one task

### 📚 Allowed Resources (Like Real Exam)
- ✅ Kubernetes official documentation
- ✅ `kubectl explain` command
- ✅ `kubectl --help` and command help
- ❌ External websites
- ❌ Chat/communication tools
- ❌ Copy-paste from external sources

## Progress Tracking

Track your scores:
- [ ] Exam 1: ___ / 100
- [ ] Exam 2: ___ / 100
- [ ] Exam 3: ___ / 100
- [ ] Exam 4: ___ / 100
- [ ] Exam 5: ___ / 100
- [ ] Exam 6: ___ / 100
- [ ] Exam 7: ___ / 100
- [ ] Exam 8: ___ / 100
- [ ] Exam 9: ___ / 100
- [ ] Exam 10: ___ / 100

**Target**: Score > 80% on all exams before taking real CKA

## 🎯 Exam Day Simulation (Most Realistic Practice)

For the most realistic practice experience:

1. 🖥️ **Use a clean Kubernetes cluster** (minikube, kind, or kubeadm)
2. ⏱️ **Set timer for exactly 2 hours** - no extensions
3. 🚫 **No external help** (except kubectl explain and official docs)
4. 📝 **Complete all tasks** - even if not perfect
5. ✅ **Verify each task** before moving on
6. 📊 **Review solutions** only after time is up
7. 📈 **Track your score** and identify weak areas

### Real Exam Environment
- Live Kubernetes cluster via PSI Bridge
- Terminal access with kubectl
- Browser access to kubernetes.io/docs only
- No copy-paste from external sources
- 2-hour time limit, strict enforcement

## Next Steps

After completing all 10 exams:
1. Review all incorrect answers
2. Practice weak areas
3. Retake exams you scored < 80%
4. Schedule real CKA exam when consistently scoring > 80%

---

**Good luck with your CKA preparation!** 🚀

