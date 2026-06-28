# 🤖 AI Code Evaluation Framework
### Automated benchmarking and guardrailing for AI-generated code quality

> A scalable, security-hardened pipeline that evaluates AI-generated code across functional correctness, security vulnerabilities, and runtime performance — outputting structured JSON reports ready for CI/CD integration.

---

## The Problem

As organisations integrate LLMs into software development workflows, a dangerous gap emerges: **no one is systematically verifying the code these models produce.** A function that passes a surface-level glance can hide SQL injection vulnerabilities, infinite loops, or silent logic failures.

This framework closes that gap with a standardised, automated evaluation pipeline.

---

## What It Does

Submit a piece of AI-generated code and a set of test cases. The framework:

1. Runs **static analysis** (AST parsing, linting, SAST) before execution
2. Spins up a **network-isolated, resource-capped sandbox** for safe execution
3. Executes code against **unit tests and property-based tests**
4. Scores across **3 evaluation pillars**
5. Returns a **structured JSON report** with grades, vulnerability details, and performance metrics

---

## Evaluation Pipeline

```
[ Code Snippet + Test Cases ]
           │
           ▼
┌─────────────────────────────────┐
│  STAGE 1 · STATIC ANALYSIS      │
│  AST Parsing · Linting · SAST   │
│  Malicious pattern detection    │
└────────────────┬────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│  STAGE 2 · SANDBOX PROVISIONING │
│  Docker + gVisor isolation      │
│  Network air-gapped             │
│  256MB RAM · 5s execution limit │
└────────────────┬────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│  STAGE 3 · DYNAMIC EXECUTION    │
│  Unit tests · Property tests    │
│  stdout / stderr capture        │
└────────────────┬────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│  STAGE 4 · METRIC AGGREGATION   │
│  Pass@k · OWASP scoring         │
│  Execution time · Memory usage  │
└────────────────┬────────────────┘
                 │
                 ▼
        [ Structured JSON Report ]
```

---

## Tech Stack

| Component | Technology |
|---|---|
| Primary Language | Python |
| Sandbox Isolation | Docker + gVisor / Firecracker microVMs |
| Task Queue | RabbitMQ / Redis |
| Static Analysis | Bandit, AST parser, dependency scanner |
| Security Standard | OWASP Top 10 |
| Output Format | Structured JSON |
| Integration | REST API + CLI · CI/CD ready |

---

## Evaluation Pillars

| Pillar | Method | Key Metrics |
|---|---|---|
| **Functional Correctness** | Unit Testing, Pass@k | Pass/Fail ratio, Coverage %, Edge-case resilience |
| **Security & Vulnerability** | SAST, Dependency Scanning | OWASP Top 10, Hardcoded secrets, Malicious imports |
| **Performance & Efficiency** | Resource Profiling | Execution time (ms), Peak memory (MB), Big-O complexity |

---

## Key Metrics

- 📋 **120+** structured evaluation tasks completed
- 🔒 **3-layer** defence-in-depth sandbox (kernel isolation + network air-gap + cgroups)
- ⏱️ **5s** hard wall-clock execution limit per sandbox
- 💾 **256MB** RAM hard cap per execution container
- 📊 **Pass@k** metric implemented for multi-sample model capability assessment

---

## Sample Output

```json
{
  "evaluation_id": "eval_8f93a2b1-4cd7",
  "timestamp": "2026-06-27T18:37:00Z",
  "language": "python",
  "status": "COMPLETED",
  "summary": {
    "functional_pass_rate": 1.0,
    "security_score": 0.95,
    "performance_grade": "A"
  },
  "metrics": {
    "functional": {
      "total_tests": 12,
      "passed": 12,
      "failed": 0,
      "coverage_percentage": 94.5
    },
    "security": {
      "vulnerabilities_found": 0,
      "warnings": [{
        "plugin": "bandit",
        "message": "Use of standard pseudo-random generators found.",
        "severity": "LOW"
      }]
    },
    "performance": {
      "execution_time_ms": 42.1,
      "peak_memory_mb": 18.4
    }
  }
}
```

---

## Security Architecture

Executing untrusted AI-generated code presents severe risks. The framework uses a **defence-in-depth** approach:

- **Kernel Isolation** — gVisor intercepts host system calls, creating a strong security boundary
- **Network Air-Gapping** — sandboxes have zero route to the internet or internal infrastructure
- **cgroups Resource Limits** — max 1 vCPU, 256MB RAM, 5s hard timeout
- **Read-Only File System** — root FS mounted read-only; only `/tmp` (in-memory) is writable
- **Instant Teardown** — container destroyed immediately after execution; no state leakage

---

## Roadmap

- [ ] LLM-in-the-Loop feedback: feed failed tests back to the model for self-correction
- [ ] Repository-level evaluation (multi-file, full codebase context)
- [ ] GPU/CUDA profiling workers for ML code evaluation

---

## About the Developer

Built by **Ayomide Sanni** — Software Engineer & AI Training Specialist based in Saskatchewan, Canada.

🌐 [Portfolio](https://ayomidesanni.github.io) · 💼 [LinkedIn](https://www.linkedin.com/in/hauwa-sanni-89292527b/) · 📧 awawsanni@gmail.com
