# CKA Exam Practice Repository

This repository contains comprehensive practice scenarios for the Certified Kubernetes Administrator (CKA) exam. Each scenario includes detailed questions, solutions, and step-by-step instructions.

## 🆕 Recent Improvements

- ✅ **Official CKA Curriculum Guide** - Complete exam breakdown with domain weights (`CKA_EXAM_CURRICULUM.md`)
- ✅ **Structure Guide** - Easy navigation guide (`STRUCTURE_GUIDE.md`)
- ✅ **Enhanced Practice Exams** - Improved format matching real CKA exam style
- ✅ **Better Documentation** - Clear instructions and verification steps

## 📚 Quick Start

1. **New to CKA?** → Read `CKA_EXAM_CURRICULUM.md` to understand the exam
2. **Need Navigation?** → Check `STRUCTURE_GUIDE.md` for repository organization
3. **Ready to Practice?** → Start with `practice-exams/EXAM_01.md`
4. **Need Quick Reference?** → Use `CHEAT_SHEET_ONE_PAGE.md`

## 📁 Repository Structure

### 🎯 Core Documentation (Start Here)
- **`CKA_EXAM_CURRICULUM.md`** - Official exam breakdown with domain weights and study strategy
- **`STRUCTURE_GUIDE.md`** - Complete navigation guide for the repository
- **`QUICK_START_GUIDE.md`** - Quick start guide for new users

### 📝 Practice Exams (Full-Length Simulations)
- **`practice-exams/`** - 10 complete 2-hour practice exams
  - `EXAM_01.md` through `EXAM_10.md` - Full practice exams
  - `EXAM_*_SOLUTIONS.md` - Complete solutions
  - `README.md` - Exam instructions and tips

### 📚 Topic-Based Practice
- **`01-core-concepts/`** - Pods, Deployments, ReplicaSets (20+ questions)
- **`02-networking/`** - Services, Ingress, NetworkPolicies (18+ questions)
- **`03-storage/`** - PV, PVC, StorageClass (15+ questions)
- **`04-security/`** - RBAC, Secrets, ServiceAccounts (17+ questions)
- **`05-troubleshooting/`** - Debugging and fixing issues (20+ questions) ⚠️ **30% of exam!**
- **`06-cluster-maintenance/`** - Upgrades, backups, node management (25+ questions)
- **`07-advanced-workloads/`** - Jobs, CronJobs, DaemonSets, StatefulSets (20+ questions)
- **`08-configuration/`** - ConfigMaps, Secrets, ResourceQuotas (20+ questions)

### 🎓 Progressive Learning Path
- **`00-basic-to-advanced/`** - Structured progression from basics to expert
  - `01-basics/` - Foundation concepts (18 questions)
  - `02-intermediate/` - Practical skills (18 questions)
  - `03-advanced/` - Complex scenarios (35 questions)
  - `04-expert/` - Mastery level (23 questions)
  - `05-exam-simulation/` - Exam simulation exercises (see `practice-exams/` for full exams)

### 📖 Study Resources
- **`SCENARIO_BASED_QUESTIONS.md`** - 50+ real exam-style scenario questions
- **`SCENARIO_SOLUTIONS_DETAILED.md`** - Complete step-by-step solutions for all scenarios
- **`INTEGRATION_SCENARIOS.md`** - 8 complex multi-step integration scenarios
- **`scenario-solutions/`** - Quick reference YAML solution files

### 📋 Reference Materials
- **`KUBERNETES_CHEAT_SHEET.md`** - Complete kubectl command cheat sheet
- **`CHEAT_SHEET_ONE_PAGE.md`** - One-page quick reference (printable)
- **`EXAM_DAY_CHECKLIST.md`** - Exam day strategy and checklist
- **`GETTING_STARTED.md`** - Step-by-step guide for beginners

## Statistics

- **180+ Individual Practice Questions** (organized by topic)
- **94 Progressive Questions** (Basic → Expert learning path)
- **50+ Scenario-Based Questions** (real exam-style)
- **8 Complete Practice Exam Scenarios**
- **8 Integration Scenarios** (complex multi-step problems)
- **10 Complete Practice Exams** (2-hour full simulations with solutions)
- **200+ Solution YAML Files**
- **Comprehensive Coverage of All CKA Exam Domains**

## New: Progressive Learning Path

**00-basic-to-advanced/** - Structured progression:
- **Level 1: Basics** (18 questions) - Foundation concepts
- **Level 2: Intermediate** (18 questions) - Practical skills
- **Level 3: Advanced** (35 questions) - Complex scenarios
- **Level 4: Expert** (23 questions) - Mastery level
- **Level 5: Exam Simulation** (3 full exams) - Exam readiness

Perfect for beginners starting from scratch or experienced users wanting structured practice!

## New: Scenario-Based Practice

**SCENARIO_BASED_QUESTIONS.md** contains 50+ realistic exam scenarios:
- **Basic Scenarios** (1-5): Foundation level, 3-6 minutes each
- **Intermediate Scenarios** (6-12): Practical level, 6-9 minutes each
- **Advanced Scenarios** (13-30): Expert level, 8-15 minutes each
- **Real Exam-Style** (31-50): Based on actual CKA patterns
- **Full Exam Simulations**: Complete 2-hour practice exams

**SCENARIO_SOLUTIONS_DETAILED.md** provides:
- Complete step-by-step solutions
- Diagnostic commands
- Common fix patterns
- YAML templates
- Troubleshooting workflows
- Time-saving tips

## How to Use

1. **Start with Basics**: Read `GETTING_STARTED.md` for setup instructions
2. **Practice by Topic**: Navigate to each numbered folder (01-08)
3. **Read Questions**: Each folder has a `README.md` with detailed questions
4. **Solve First**: Try to solve problems yourself before checking solutions
5. **Check Solutions**: Review YAML files in `solutions/` folder
6. **Apply & Verify**: Apply solutions to your cluster and verify they work
7. **Practice Exams**: Complete full exams in `practice-exams/` directory
8. **Integration Scenarios**: Challenge yourself with `INTEGRATION_SCENARIOS.md`
9. **Scenario-Based Practice**: Work through `SCENARIO_BASED_QUESTIONS.md` (50+ real exam scenarios)
10. **Quick Reference**: Keep `CHEAT_SHEET_ONE_PAGE.md` and `KUBERNETES_CHEAT_SHEET.md` handy
11. **Exam Day Prep**: Review `EXAM_DAY_CHECKLIST.md` before your exam
12. **Command Reference**: Use `KUBERNETES_CHEAT_SHEET.md` for quick command lookup during practice

## Prerequisites

- A Kubernetes cluster (minikube, kind, or cloud-based)
- kubectl configured and working
- Basic understanding of Kubernetes concepts

## Exam Tips

- Practice with kubectl imperative commands (faster for exam)
- Know how to use `kubectl explain` for help
- Practice with `--dry-run=client -o yaml` to generate YAML
- Time management is crucial (2 hours for 15-20 questions)
- Use `kubectl get` with various output formats

Good luck with your CKA exam preparation!

