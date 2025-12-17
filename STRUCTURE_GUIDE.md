# CKA Practice Repository Structure Guide

## 📁 Repository Organization

This repository is organized to match the official CKA exam curriculum and make studying easier.

```
kubernetes_practice/
│
├── 📘 CKA_EXAM_CURRICULUM.md          # Official exam breakdown & weights
├── 📘 STRUCTURE_GUIDE.md              # This file - navigation guide
├── 📘 README.md                       # Main repository overview
│
├── 📂 practice-exams/                 # Full-length practice exams
│   ├── README.md                      # Exam instructions & tips
│   ├── EXAM_01.md                     # Practice Exam 1
│   ├── EXAM_01_SOLUTIONS.md           # Solutions for Exam 1
│   ├── EXAM_02.md                     # Practice Exam 2
│   ├── EXAM_03.md                     # ... (up to EXAM_10)
│   └── ...
│
├── 📂 domain-practice/                 # Practice by CKA exam domain
│   │
│   ├── 📂 01-cluster-architecture/     # 25% of exam
│   │   ├── README.md                  # Domain overview & topics
│   │   ├── questions/                 # Domain-specific questions
│   │   └── solutions/                 # Solutions
│   │
│   ├── 📂 02-workloads-scheduling/     # 15% of exam
│   │   ├── README.md
│   │   ├── questions/
│   │   └── solutions/
│   │
│   ├── 📂 03-services-networking/      # 20% of exam
│   │   ├── README.md
│   │   ├── questions/
│   │   └── solutions/
│   │
│   ├── 📂 04-storage/                  # 10% of exam
│   │   ├── README.md
│   │   ├── questions/
│   │   └── solutions/
│   │
│   └── 📂 05-troubleshooting/          # 30% of exam (LARGEST)
│       ├── README.md
│       ├── questions/
│       └── solutions/
│
├── 📂 topic-practice/                  # Practice by specific topics
│   ├── 📂 core-concepts/               # Pods, Deployments, Services
│   ├── 📂 networking/                  # Services, Ingress, NetworkPolicies
│   ├── 📂 storage/                     # PVs, PVCs, StorageClasses
│   ├── 📂 security/                    # RBAC, Secrets, Pod Security
│   ├── 📂 troubleshooting/             # Debugging scenarios
│   ├── 📂 cluster-maintenance/         # Upgrades, backups, etcd
│   ├── 📂 advanced-workloads/          # StatefulSets, DaemonSets, Jobs
│   └── 📂 configuration/               # ConfigMaps, Secrets, etc.
│
├── 📂 scenario-solutions/              # Complete scenario solutions
│   └── *.yaml                          # Solution manifests
│
└── 📂 study-resources/                 # Study aids
    ├── CHEAT_SHEET_ONE_PAGE.md
    ├── KUBERNETES_CHEAT_SHEET.md
    ├── QUICK_REFERENCE.md
    ├── CKA_QUICK_START.md
    ├── CKA_STUDY_SCHEDULE.md
    ├── EXAM_DAY_CHECKLIST.md
    └── PRACTICE_EXAM_TRACKER.md
```

## 🎯 How to Use This Repository

### For Beginners
1. Start with `CKA_EXAM_CURRICULUM.md` to understand the exam
2. Practice in `topic-practice/` starting with `core-concepts/`
3. Progress through topics in order
4. Move to `domain-practice/` when comfortable
5. Take full exams in `practice-exams/` when ready

### For Intermediate Learners
1. Review `CKA_EXAM_CURRICULUM.md` for domain weights
2. Focus on weak domains in `domain-practice/`
3. Practice specific topics in `topic-practice/`
4. Take practice exams regularly
5. Review solutions and improve

### For Exam Preparation
1. Take all practice exams in `practice-exams/`
2. Focus heavily on `domain-practice/05-troubleshooting/` (30%)
3. Review `domain-practice/01-cluster-architecture/` (25%)
4. Practice `domain-practice/03-services-networking/` (20%)
5. Use study resources for quick reference

## 📊 Exam Domain Weights

| Domain | Weight | Priority | Practice Location |
|--------|--------|----------|-------------------|
| Troubleshooting | 30% | 🔴 Critical | `domain-practice/05-troubleshooting/` |
| Cluster Architecture | 25% | 🔴 Critical | `domain-practice/01-cluster-architecture/` |
| Services & Networking | 20% | 🟡 High | `domain-practice/03-services-networking/` |
| Workloads & Scheduling | 15% | 🟢 Medium | `domain-practice/02-workloads-scheduling/` |
| Storage | 10% | 🟢 Medium | `domain-practice/04-storage/` |

## 🗺️ Navigation Tips

### Finding Questions by Topic
- **Core Kubernetes concepts**: `topic-practice/core-concepts/`
- **Networking issues**: `topic-practice/networking/` or `domain-practice/03-services-networking/`
- **Storage problems**: `topic-practice/storage/` or `domain-practice/04-storage/`
- **RBAC/Security**: `topic-practice/security/`
- **Troubleshooting**: `domain-practice/05-troubleshooting/` (most important!)

### Finding Full Exams
- All full-length exams: `practice-exams/`
- Solutions: Each exam has a corresponding `*_SOLUTIONS.md` file

### Finding Solutions
- Domain solutions: `domain-practice/[domain]/solutions/`
- Topic solutions: `topic-practice/[topic]/solutions/`
- Scenario solutions: `scenario-solutions/*.yaml`
- Exam solutions: `practice-exams/EXAM_*_SOLUTIONS.md`

## 📝 Study Workflow

### Daily Practice
1. Pick a domain from `domain-practice/`
2. Read the README for that domain
3. Attempt questions
4. Review solutions
5. Practice commands hands-on

### Weekly Review
1. Take a full practice exam
2. Review all incorrect answers
3. Focus on weak domains
4. Update progress tracker

### Pre-Exam
1. Review all cheat sheets
2. Take multiple practice exams
3. Focus on troubleshooting (30%)
4. Review exam day checklist

## 🔍 Quick Reference

- **Need exam overview?** → `CKA_EXAM_CURRICULUM.md`
- **Need quick commands?** → `study-resources/CHEAT_SHEET_ONE_PAGE.md`
- **Need full exam?** → `practice-exams/EXAM_01.md`
- **Need domain practice?** → `domain-practice/[domain]/`
- **Need topic practice?** → `topic-practice/[topic]/`
- **Need solutions?** → Check `solutions/` folders or `*_SOLUTIONS.md`

## ✅ Progress Tracking

Track your progress:
- [ ] Reviewed `CKA_EXAM_CURRICULUM.md`
- [ ] Completed all domain practice
- [ ] Completed all topic practice
- [ ] Scored >80% on all practice exams
- [ ] Reviewed exam day checklist
- [ ] Ready for real CKA exam!

---

**Last Updated**: 2024
**Repository Purpose**: Comprehensive CKA exam preparation with organized, exam-like questions and solutions

