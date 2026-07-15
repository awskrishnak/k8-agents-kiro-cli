# K8s Agents for Kiro CLI

A collection of 36 specialized Kubernetes operations agents for [Kiro CLI](https://kiro.dev/cli/). These agents provide expert-level debugging, security review, incident management, and infrastructure audit capabilities — all read-only by default.

## What This Is

A drop-in agent toolkit that turns Kiro CLI into a Kubernetes operations copilot. Each agent is a focused specialist (pod debugging, network tracing, RBAC auditing, etc.) orchestrated by a single entry point that triages problems and routes to the right expert.

## Quick Start

### Installation

Copy the contents to your Kiro CLI steering directory:

```bash
# Clone the repo
git clone https://github.com/awskrishnak/k8-agents-kiro-cli.git

# Copy to your Kiro CLI steering directory
cp -r k8-agents-kiro-cli/ ~/.kiro/steering/k8s-ops/
```

### Usage

Start Kiro CLI and use the k8s-ops agent:

```bash
kiro-cli chat --agent k8s-ops
```

Or switch mid-session:

```
/agent k8s-ops
```

Then just describe your problem:

- *"Pod nginx-xyz is CrashLoopBackOff"*
- *"Can't reach service from another namespace"*
- *"PVC stuck in Pending"*
- *"Run a full cluster audit"*

The ops-lead agent will triage and route to the correct specialist automatically.

## Agents (36 Total)

### Debugging (11)

| Agent | Use When |
|-------|----------|
| pod-debugger | Pod is Pending, restarting, or terminating |
| network-debugger | Service unreachable, DNS issues, Ingress 5xx |
| storage-investigator | PVC Pending, mount failures, CSI errors |
| probe-debugger | Liveness/readiness probe failures |
| statefulset-debugger | Ordered startup issues, PVC binding |
| job-debugger | Job/CronJob never completes |
| operator-debugger | CRD stuck, controller not reconciling |
| cni-debugger | CNI errors, IP exhaustion |
| mesh-debugger | Service mesh traffic failures (Istio/Linkerd/Cilium) |
| certificate-debugger | TLS handshake failures, cert-manager issues |
| performance-profiler | CPU throttling, memory pressure, I/O bottlenecks |

### Security & Compliance (4)

| Agent | Use When |
|-------|----------|
| security-reviewer | Pre-merge manifest/policy review |
| rbac-auditor | Access denials or excessive permissions |
| image-auditor | CVE scanning, image policy compliance |
| admission-debugger | Webhook denials, mutation failures |

### Resource Management (5)

| Agent | Use When |
|-------|----------|
| capacity-planner | Scheduling failures, headroom analysis |
| quota-auditor | ResourceQuota/LimitRange denials |
| pdb-investigator | Node drain stuck, autoscaler blocked |
| autoscaler-investigator | HPA/VPA/CA not scaling |
| finops-reviewer | Cost anomalies, over-provisioning |

### Observability & Events (3)

| Agent | Use When |
|-------|----------|
| event-analyzer | Event storms, failure pattern correlation |
| config-differ | ConfigMap/Secret drift detection |
| etcd-investigator | Control plane slow, API timeouts |

### Infrastructure & GitOps (4)

| Agent | Use When |
|-------|----------|
| terraform-reviewer | Review Terraform/OpenTofu plans |
| helm-reviewer | Chart template defects |
| gitops-reviewer | ArgoCD/Flux out-of-sync |
| deployment-verifier | Post-deploy health check |

### Incident Management (3)

| Agent | Use When |
|-------|----------|
| sre-investigator | Active incident signal correlation |
| incident-historian | Build incident timeline |
| postmortem-writer | Draft RCA documents |

### Operations (4)

| Agent | Use When |
|-------|----------|
| cluster-explorer | Map cluster structure |
| multi-cluster-mapper | Fleet-wide consistency checks |
| backup-verifier | Backup schedule health |
| documentation-agent | Update runbooks after changes |

### Orchestration (2)

| Agent | Use When |
|-------|----------|
| ops-lead | Default entry point — triages and routes |
| agent-memory-protocol | Cross-session knowledge persistence |

## Key Principles

1. **Read-only by default** — No agent modifies cluster state
2. **Evidence-based** — Facts, hypotheses, and missing evidence clearly separated
3. **Turn-limited** — Each agent has a turn budget; partial results reported if exhausted
4. **Secret-safe** — Never decodes or prints Secret values; reference by name only
5. **No assumption of root cause** — Hypotheses ranked by likelihood, never confirmed without direct evidence

## Full Cluster Audit

Ask the ops-lead to run a comprehensive audit:

```
Run a full cluster audit
```

It executes a 6-phase pipeline across all agents and produces an Excel report with:
- Executive Summary
- Security Findings
- Resource Capacity
- Workload Health
- Configuration Audit
- Network & Storage
- Recommendations

## Prerequisites

- `kubectl` configured with cluster context
- Read access to cluster resources (get/list/watch)
- For infrastructure agents: `terraform`/`tofu`, `helm` CLI
- For mesh debugging: `istioctl`, `linkerd`, or `cilium` CLI

## Directory Structure

```
k8s-ops/
├── SKILL.md                    # Main skill definition
├── kk-brand.md                 # Brand styling for reports
├── README.md
├── LICENSE
└── agents/
    ├── ops-lead.md             # Orchestrator / entry point
    ├── agent-memory-protocol.md
    ├── pod-debugger.md
    ├── network-debugger.md
    ├── storage-investigator.md
    ├── ... (36 agent files)
    └── documentation-agent.md
```

## Customization

### Adding Your Own Agents

Create a new `.md` file in `agents/` following this structure:

```markdown
---
name: my-agent
description: One-line description for routing.
---

# My Agent

You are a [role]. You [scope] — strictly read-only.

## Method
1. Step one...
2. Step two...

## Boundary
- No writes: ...
- Stop at N turns.

## Output contract
Return exactly:
1. ...
2. ...
```

Then add the routing entry in `agents/ops-lead.md` and `SKILL.md`.

### Brand Customization

Edit `kk-brand.md` to match your organization's colors, fonts, and report formatting standards.

## Contributing

Issues and PRs welcome. Please:
- Keep agents read-only by default
- Include clear boundary rules
- Add memory usage patterns for cross-session recall
- Test with real cluster scenarios

## License

MIT License — see [LICENSE](./LICENSE)

## Author

Built by [KK](https://github.com/awskrishnak) for the Kiro CLI community.
