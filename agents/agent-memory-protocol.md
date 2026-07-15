---
name: agent-memory-protocol
description: Memory protocol for all k8s-ops agents. Defines how agents store and retrieve findings using the knowledge tool for cross-session and cross-agent persistence.
---

# Agent Memory Protocol

All k8s-ops agents MUST use the `knowledge` tool for persistent memory. This enables:
- Cross-session recall (findings persist between chat sessions)
- Cross-agent sharing (k8s-ops lead can retrieve any sub-agent's findings)
- Pattern recognition (recurring issues detected across audits)

## When to Store (knowledge add)

Store findings in these scenarios:
1. **Confirmed root causes** — after investigation confirms a cause
2. **Recurring patterns** — when the same issue appears across multiple resources
3. **Cluster-specific context** — topology, installed tools, CNI type, mesh type, StorageClasses
4. **Audit findings** — severity-rated findings from any investigation
5. **Resolution history** — what fixed similar issues before

## Naming Convention

Use this pattern for knowledge entries:
```
k8s-{agent-name}-{cluster-context}-{category}
```

Examples:
- `k8s-pod-debugger-prod-cluster-crashloops`
- `k8s-network-debugger-staging-dns-failures`
- `k8s-cluster-audit-2026-07-findings`
- `k8s-security-reviewer-rbac-gaps`

## When to Search (knowledge search)

Search BEFORE starting investigation:
1. Check for prior findings on the same resource/namespace
2. Check for known cluster-specific constraints
3. Check for previously confirmed patterns matching current symptoms

## What to Store

```
knowledge add:
  name: "k8s-{agent}-{context}-{topic}"
  value: |
    ## Finding: {one-line summary}
    Date: {timestamp}
    Cluster: {context name}
    Severity: {Critical/High/Medium/Low}
    Resource: {namespace/name}
    Evidence: {key evidence}
    Root Cause: {confirmed or hypothesis}
    Resolution: {what fixed it, or recommended fix}
    Pattern: {if recurring, describe the pattern}
```

## k8s-ops Lead Memory Usage

The ops-lead orchestrator uses memory to:
1. **Before routing** — search for prior findings on same resource/namespace to provide context to sub-agents
2. **After synthesis** — store combined findings for future reference
3. **Pattern detection** — query across all agent findings to identify systemic issues
4. **Audit history** — maintain running record of cluster health over time

## Memory Hygiene

- Store concise findings, not full log dumps
- Update existing entries rather than creating duplicates
- Include timestamps for temporal correlation
- Tag with cluster context for multi-cluster environments
- Mark resolved findings as "RESOLVED" with resolution date
