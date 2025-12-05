# CKA Exam Practice Scenarios

This repository contains comprehensive practice scenarios for the Certified Kubernetes Administrator (CKA) exam. Each scenario includes detailed questions, solutions, and step-by-step instructions.

## Structure

```
├── 01-core-concepts/          # Pods, Deployments, ReplicaSets (20+ questions)
├── 02-networking/             # Services, Ingress, NetworkPolicies (18+ questions)
├── 03-storage/                # PV, PVC, StorageClass (15+ questions)
├── 04-security/               # RBAC, Secrets, ServiceAccounts (17+ questions)
├── 05-troubleshooting/        # Debugging and fixing issues (20+ questions)
├── 06-cluster-maintenance/    # Upgrades, backups, node management (25+ questions)
├── 07-advanced-workloads/     # Jobs, CronJobs, DaemonSets, StatefulSets (20+ questions)
├── 08-configuration/          # ConfigMaps, Secrets, ResourceQuotas (20+ questions)
├── 00-basic-to-advanced/      # Progressive learning path (Basic → Expert)
│   ├── 01-basics/             # Foundation concepts (18 questions)
│   ├── 02-intermediate/       # Practical skills (18 questions)
│   ├── 03-advanced/           # Complex scenarios (35 questions)
│   ├── 04-expert/             # Mastery level (23 questions)
│   └── 05-exam-simulation/    # Full exam practice (3 exams)
├── practice-exams/            # 10 Complete CKA Practice Exams
│   ├── EXAM_01.md             # Foundation exam
│   ├── EXAM_01_SOLUTIONS.md   # Complete solutions
│   ├── EXAM_02.md through EXAM_10.md
│   └── README.md              # Exam guide and tips
├── PRACTICE_EXAM.md           # 8 complete practice exam scenarios
├── INTEGRATION_SCENARIOS.md   # 8 complex multi-step integration scenarios
├── SCENARIO_BASED_QUESTIONS.md # 50+ real exam-style scenario questions
├── SCENARIO_SOLUTIONS.md      # Quick solutions and troubleshooting patterns
├── SCENARIO_SOLUTIONS_DETAILED.md # Complete step-by-step solutions for all scenarios
├── EXAM_DAY_CHECKLIST.md      # Exam day strategy and checklist
├── KUBERNETES_CHEAT_SHEET.md  # Complete kubectl command cheat sheet
├── CHEAT_SHEET_ONE_PAGE.md    # One-page quick reference (printable)
├── scenario-solutions/        # Quick reference YAML solution files
├── QUICK_REFERENCE.md         # Essential kubectl commands and YAML templates
├── QUESTIONS_SUMMARY.md        # Complete overview of all questions
└── GETTING_STARTED.md         # Step-by-step guide
```

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

**SCENARIO_SOLUTIONS.md** provides:
- Quick diagnostic commands
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
7. **Practice Exams**: Complete scenarios in `PRACTICE_EXAM.md`
8. **Integration Scenarios**: Challenge yourself with `INTEGRATION_SCENARIOS.md`
9. **Scenario-Based Practice**: Work through `SCENARIO_BASED_QUESTIONS.md` (50+ real exam scenarios)
10. **Quick Reference**: Keep `QUICK_REFERENCE.md`, `SCENARIO_SOLUTIONS.md`, and `KUBERNETES_CHEAT_SHEET.md` handy
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

