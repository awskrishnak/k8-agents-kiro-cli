---
name: k8s-ops
description: "Kubernetes operations toolkit: 35 specialized agents for cluster auditing, debugging, security review, incident management, and infrastructure review. Covers pod debugging, network troubleshooting, RBAC auditing, storage investigation, service mesh, certificates, autoscaling, capacity planning, backups, GitOps, Terraform, Helm, FinOps, and comprehensive cluster audits with Excel reporting."
argument-hint: "[operation] [context]"
license: MIT
metadata:
  author: kk
  version: "1.0.0"
---

# Kubernetes Operations Toolkit

35 specialized agents for Kubernetes cluster operations, debugging, security, and audit.

## When to Use

- Debugging failing Pods, networking, storage, or probes
- Security reviews of manifests, RBAC, images, or admission control
- Cluster audits (comprehensive or targeted)
- Incident investigation and postmortem writing
- Infrastructure review (Terraform, Helm, GitOps)
- Capacity planning and FinOps cost analysis
- Service mesh, certificate, and autoscaler troubleshooting

## Entry Points

### k8s-ops (Single Entry Point)

The **one agent** for all Kubernetes operations. It triages the problem, routes to the correct specialist agent(s), coordinates multi-agent investigations, synthesizes results, and runs full cluster audits.

Reference: `agents/ops-lead.md`

Use for:
- Any K8s problem (it routes automatically)
- Multi-agent coordination
- Incident response
- Full cluster audits with Excel report
- Pre-deploy reviews

## Agent Routing

| Scenario | Agent | Reference |
|----------|-------|-----------|
| **Any K8s operations request (default)** | **k8s-ops** | `agents/ops-lead.md` |
| Pod CrashLoopBackOff/Pending | pod-debugger | `agents/pod-debugger.md` |
| Network/DNS/Ingress failure | network-debugger | `agents/network-debugger.md` |
| Storage/PVC/CSI issues | storage-investigator | `agents/storage-investigator.md` |
| Probe failures (liveness/readiness) | probe-debugger | `agents/probe-debugger.md` |
| StatefulSet ordered startup issues | statefulset-debugger | `agents/statefulset-debugger.md` |
| Job/CronJob failures | job-debugger | `agents/job-debugger.md` |
| Custom operator/CRD issues | operator-debugger | `agents/operator-debugger.md` |
| RBAC/IAM access denials | rbac-auditor | `agents/rbac-auditor.md` |
| Security manifest review | security-reviewer | `agents/security-reviewer.md` |
| Container image CVEs/policy | image-auditor | `agents/image-auditor.md` |
| Admission webhook denials | admission-debugger | `agents/admission-debugger.md` |
| Certificate/TLS failures | certificate-debugger | `agents/certificate-debugger.md` |
| CNI/IP allocation failures | cni-debugger | `agents/cni-debugger.md` |
| Service mesh traffic issues | mesh-debugger | `agents/mesh-debugger.md` |
| HPA/VPA/cluster autoscaler | autoscaler-investigator | `agents/autoscaler-investigator.md` |
| Capacity planning/headroom | capacity-planner | `agents/capacity-planner.md` |
| PodDisruptionBudget blocking | pdb-investigator | `agents/pdb-investigator.md` |
| ResourceQuota exhaustion | quota-auditor | `agents/quota-auditor.md` |
| Event storms/correlation | event-analyzer | `agents/event-analyzer.md` |
| ConfigMap/Secret drift | config-differ | `agents/config-differ.md` |
| etcd/control plane health | etcd-investigator | `agents/etcd-investigator.md` |
| Performance/CPU throttling | performance-profiler | `agents/performance-profiler.md` |
| Backup/Velero health | backup-verifier | `agents/backup-verifier.md` |
| Multi-cluster fleet mapping | multi-cluster-mapper | `agents/multi-cluster-mapper.md` |
| Cluster structure mapping | cluster-explorer | `agents/cluster-explorer.md` |
| Terraform/OpenTofu review | terraform-reviewer | `agents/terraform-reviewer.md` |
| Helm chart review | helm-reviewer | `agents/helm-reviewer.md` |
| GitOps drift detection | gitops-reviewer | `agents/gitops-reviewer.md` |
| FinOps cost analysis | finops-reviewer | `agents/finops-reviewer.md` |
| Post-deployment health check | deployment-verifier | `agents/deployment-verifier.md` |
| Incident signal correlation | sre-investigator | `agents/sre-investigator.md` |
| Incident timeline building | incident-historian | `agents/incident-historian.md` |
| Postmortem/RCA writing | postmortem-writer | `agents/postmortem-writer.md` |
| Documentation updates | documentation-agent | `agents/documentation-agent.md` |

## How to Use

### Default: Use k8s-ops

```bash
kiro-cli chat --agent k8s-ops
```

Or switch mid-session: `/agent k8s-ops`

It will:
1. Triage the problem from your description
2. Select the right specialist agent(s)
3. Coordinate multi-agent investigations if needed
4. Synthesize findings into an actionable answer

Examples:
- "Pod nginx-xyz is CrashLoopBackOff" → routes to pod-debugger
- "Can't reach service from another namespace" → routes to network-debugger
- "PVC stuck in Pending" → routes to storage-investigator
- "Run a full cluster audit" → runs all agents in 6 phases, generates Excel report
- "Service intermittently failing" → runs network + pod + certificate debuggers in parallel

### Direct Specialist Access

If you know exactly which agent you need:

```bash
kiro-cli chat --agent k8s-pod-debugger
kiro-cli chat --agent k8s-network-debugger
kiro-cli chat --agent k8s-security-reviewer
```

### Multi-Agent Workflows

#### Security Review Pipeline
```
security-reviewer → image-auditor → rbac-auditor → admission-debugger
```

#### Incident Response Chain
```
sre-investigator → [targeted debugger] → incident-historian → postmortem-writer
```

#### Infrastructure Review
```
terraform-reviewer → security-reviewer → [apply] → deployment-verifier
```

## Agent Categories

### Debugging (11 agents)
Pod, network, storage, probe, StatefulSet, Job, operator, CNI, mesh, certificate, performance

### Security & Compliance (4 agents)
Security reviewer, RBAC auditor, image auditor, admission debugger

### Resource Management (5 agents)
Capacity planner, quota auditor, PDB investigator, autoscaler investigator, finops reviewer

### Observability & Events (3 agents)
Event analyzer, config differ, etcd investigator

### Infrastructure & GitOps (4 agents)
Terraform reviewer, Helm reviewer, GitOps reviewer, deployment verifier

### Incident Management (3 agents)
SRE investigator, incident historian, postmortem writer

### Operations (4 agents)
Cluster explorer, multi-cluster mapper, backup verifier, documentation agent

## Core Principles

All agents follow these rules:

1. **Read-only by default** — No agent modifies cluster state unless explicitly stated
2. **Evidence-based** — Facts, hypotheses, and missing evidence are clearly separated
3. **Turn-limited** — Each agent has a turn budget; partial results are reported if budget exhausted
4. **No assumption of root cause** — Hypotheses are ranked by likelihood, never stated as confirmed without direct evidence
5. **Safe validation only** — Recommended next steps are read-only commands for the engineer to verify
6. **Secret-safe** — Never decode or print Secret values; reference by name only

## Prerequisites

- `kubectl` configured with appropriate cluster context
- Read access to cluster resources (most agents need only get/list/watch)
- For infrastructure agents: `terraform`/`tofu`, `helm` CLI available
- For mesh debugging: mesh-specific CLI (istioctl, linkerd, cilium)
