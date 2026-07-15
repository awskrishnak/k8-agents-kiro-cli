# Tools & Commands Reference

Every command listed below is **read-only**. No agent in this toolkit modifies cluster state, infrastructure, or external systems.

---

## Common Tools (All Agents)

| Tool | Purpose |
|------|---------|
| `knowledge` (Kiro MCP) | Persistent memory — store/retrieve findings across sessions |
| `kubectl` | Primary Kubernetes API interaction (get, describe, logs, events) |

---

## Per-Agent Tool & Command Matrix

### pod-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl describe pod <name>` | Pod status, conditions, events |
| `kubectl logs <pod> [-c container]` | Container logs (current and previous) |
| `kubectl get events --field-selector involvedObject.name=<pod>` | Pod-specific events |
| `kubectl describe node <node>` | Node conditions, capacity, allocatable |
| `kubectl get pod -o yaml` | Full Pod spec for analysis |

---

### network-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl get/describe svc,endpoints,ingress,networkpolicy` | Service routing chain |
| `dig` / `nslookup` | DNS resolution testing (from debug context) |
| `kubectl get endpoints <svc>` | Verify backend Pod IPs |
| ALB/target-group describe (AWS CLI) | Load balancer health and routing rules |
| `kubectl describe ingress` | Ingress controller rules and backends |

**Never runs:** `kubectl exec` into production containers (unless policy-permitted debug Pod)

---

### storage-investigator

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl describe pvc <name>` | PVC status, binding, events |
| `kubectl describe pv <name>` | PV capacity, access mode, reclaim policy |
| `kubectl describe storageclass` | Provisioner, parameters, volume binding |
| `kubectl get volumeattachment` | Node attachment state |
| `kubectl describe pod` | Volume mount errors in Pod events |

---

### probe-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl describe pod` | Probe config (type, endpoint, timeouts) |
| `kubectl logs <pod>` | Application logs at probe failure time |
| Kubelet logs (node-level) | Probe execution results, response codes |
| `kubectl get events` | Probe failure events with timestamps |

---

### statefulset-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl describe statefulset` | Replicas, rollout status, update strategy |
| `kubectl get pvc -l <statefulset-labels>` | Per-ordinal PVC status |
| `kubectl describe pvc` | Binding state, StorageClass |
| `kubectl get pod -l <selector> --sort-by=.metadata.name` | Ordered Pod list |
| DNS lookup: `pod-N.svc.ns.svc.cluster.local` | Headless service identity verification |

---

### job-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl describe job/cronjob` | Status, completions, parallelism, backoffLimit |
| `kubectl get pods --selector=job-name=<job>` | Pods created by Job |
| `kubectl logs <job-pod>` | Exit codes and failure output |
| `kubectl get events` | Schedule triggers, creation events |

---

### operator-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl describe <custom-resource>` | CR status.conditions, observedGeneration |
| `kubectl logs <operator-pod>` | Controller reconciliation errors |
| `kubectl describe deployment <operator>` | Operator Pod health, restarts |
| `kubectl get crd <name> -o yaml` | CRD schema, versions |

---

### cni-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl get daemonset -n kube-system` | CNI DaemonSet health per node |
| `kubectl logs <cni-pod> -n kube-system` | CNI plugin errors (node-local) |
| CNI plugin status commands | IP pool allocation, subnet state |
| IP allocation queries | Available vs allocated IPs |
| `ping` / `traceroute` (from debug Pod) | Cross-node encapsulation testing |

---

### mesh-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `istioctl analyze` | Istio configuration validation |
| `istioctl proxy-status` | Envoy sidecar config sync state |
| `linkerd check` | Linkerd control plane health |
| `linkerd tap` | Live traffic inspection through proxy |
| `cilium status` | Cilium agent and connectivity status |
| Envoy admin interface queries (read-only) | Proxy config dump, clusters, listeners |

**Never runs:** Sidecar restarts, config reloads

---

### certificate-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl describe certificate` | Certificate Ready status, conditions |
| `kubectl describe certificaterequest` | Request status, issuer reference |
| `kubectl describe issuer/clusterissuer` | ACME account, CA status |
| `openssl s_client -connect <host>:<port>` | TLS handshake inspection |
| `openssl x509 -text` | Certificate SANs, expiry, chain |
| DNS lookup tools | ACME challenge record verification |

---

### performance-profiler

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl top pod` | CPU/memory usage per container |
| `kubectl top node` | Node-level resource consumption |
| Node stats / cgroup metrics | Throttling time, I/O wait |
| `kubectl describe pod` | Resource requests/limits |
| `kubectl get pod -o yaml` | Container resource spec |

---

### security-reviewer

| Tool / Command | Purpose |
|----------------|---------|
| Image scanners (Trivy, Grype) | CVE scan results |
| Policy-as-code tools (OPA, Kyverno) | Policy violation output |
| IaC static analysis tools | Infrastructure security findings |
| `kubectl get <manifests> -o yaml` | Manifest inspection |

**Output requires:** Explicit human approval before any remediation

---

### rbac-auditor

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl auth can-i --as=<subject>` | Permission simulation |
| `kubectl get role/clusterrole -o yaml` | Role rules inspection |
| `kubectl get rolebinding/clusterrolebinding -o yaml` | Binding → subject chain |
| IAM policy simulation (cloud) | Cloud IAM read-only queries |

---

### image-auditor

| Tool / Command | Purpose |
|----------------|---------|
| `skopeo inspect` | Image manifest, layers, labels |
| `crane manifest` / `crane digest` | Image provenance, digest |
| Scanner CLI (Trivy, Grype) | CVE results per image |
| Signature verification tools (cosign) | Image signature validation |

---

### admission-debugger

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl get validatingwebhookconfiguration -o yaml` | Webhook rules, selectors |
| `kubectl get mutatingwebhookconfiguration -o yaml` | Mutation webhook config |
| `kubectl logs <webhook-pod>` | Webhook service deny reasons |
| `kubectl describe <denied-resource>` | Admission denial events |

---

### capacity-planner

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl top node` | Actual node resource usage |
| `kubectl describe node` | Allocatable, capacity, system-reserved |
| `kubectl get pods --all-namespaces -o json` | Aggregate requests/limits |
| Resource query commands | Per-node requested vs limit totals |

---

### quota-auditor

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl describe resourcequota` | Used vs hard limits per resource |
| `kubectl describe limitrange` | Default/min/max constraints |
| `kubectl get pods -n <ns> -o json` | Per-namespace resource sum |
| `kubectl get events` | Quota denial events |

---

### pdb-investigator

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl get pdb -o yaml` | PDB spec and status |
| `kubectl describe pdb` | CurrentHealthy, desiredHealthy, disruptionsAllowed |
| `kubectl get pods -l <selector>` | Pod count and readiness |
| `kubectl get events` | Eviction blocked events |

---

### autoscaler-investigator

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl describe hpa` | Current/target metrics, replicas, conditions |
| `kubectl get --raw /apis/metrics.k8s.io/` | Metrics-server availability |
| `kubectl describe vpa` | VPA recommendations, mode |
| Cluster autoscaler logs | Node provisioning attempts |
| Cloud provider read-only API | Instance limits, ASG status |

---

### event-analyzer

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl get events --sort-by=.lastTimestamp` | Chronological event list |
| `kubectl get events --field-selector reason=<reason>` | Filtered by reason |
| `kubectl get events -n <namespace>` | Namespace-scoped events |
| `kubectl get events --all-namespaces` | Cluster-wide event view |

---

### config-differ

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl get configmap <name> -o yaml` | ConfigMap data extraction |
| `kubectl get secret <name> -o yaml` | Secret key presence (hash only, never decoded) |
| `kubectl get pods -l <selector>` | Mounted config consumers |
| `kubectl describe pod` | Volume mount timestamps, restart count |

---

### etcd-investigator

| Tool / Command | Purpose |
|----------------|---------|
| `etcdctl endpoint health` | Cluster member health |
| `etcdctl endpoint status` | Leader, DB size, revision |
| API server metrics scraping | Request latency, QPS by verb/resource |
| `etcdctl alarm list` | Active alarms (NOSPACE) |

**Never runs:** `etcdctl defrag`, `etcdctl snapshot`, member operations

---

### backup-verifier

| Tool / Command | Purpose |
|----------------|---------|
| `velero backup get` | Backup list, status, timestamps |
| `velero backup describe <name>` | Backup details, volume snapshots |
| `velero schedule get` | Schedule health, next run |
| Cloud-provider snapshot list | Snapshot status, size, location |

**Never runs:** `velero backup create`, `velero restore create`

---

### multi-cluster-mapper

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl --context=<ctx> get <resources>` | Cross-cluster resource queries |
| `kubectl config get-contexts` | Cluster inventory |
| `kubectl get nodes --context=<ctx>` | Per-cluster node info |
| `kubectl get storageclass/networkpolicy/psp` | Policy comparison across clusters |

---

### cluster-explorer

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl get all -n <namespace>` | Namespace resource inventory |
| `kubectl get namespaces` | Namespace listing |
| `kubectl get <resource> -o json` | Owner reference chains |
| `kubectl get events` | Health signal flags |

---

### terraform-reviewer

| Tool / Command | Purpose |
|----------------|---------|
| `terraform plan` / `tofu plan` | Planned changes diff (read-only) |
| `terraform validate` | Configuration syntax check |
| `terraform fmt -check` | Formatting compliance |
| `terraform state list` | State read operations |

**Never runs:** `terraform apply`, `terraform destroy`, `terraform import`

---

### helm-reviewer

| Tool / Command | Purpose |
|----------------|---------|
| `helm template <chart>` | Render YAML without cluster access |
| `helm lint <chart>` | Chart validation and warnings |
| `helm template --values <file>` | Render with specific values |

**Never runs:** `helm install`, `helm upgrade`, `helm rollback`

---

### gitops-reviewer

| Tool / Command | Purpose |
|----------------|---------|
| GitHub MCP tools | Pull declared manifest from Git ref |
| Argo CD MCP tools | Pull live resource state, sync status |
| Diff tools | Field-by-field manifest comparison |

**Never runs:** `argocd app sync`, `argocd app rollback`, `flux reconcile`

---

### finops-reviewer

| Tool / Command | Purpose |
|----------------|---------|
| Cost MCP tools (AWS Cost Explorer) | Cost data by service/resource |
| Observability MCP tools | Utilization metrics (CPU/memory) |
| `kubectl top pod/node` | Current resource usage |
| `kubectl get pods -o json` | Request/limit vs actual usage comparison |

---

### deployment-verifier

| Tool / Command | Purpose |
|----------------|---------|
| `kubectl rollout status` | Deployment rollout progress |
| `kubectl get pods -l <selector>` | New replica readiness |
| Observability MCP tools | Error rate, latency post-deploy |
| `kubectl describe deployment` | Image tag, replica count |

---

### sre-investigator

| Tool / Command | Purpose |
|----------------|---------|
| Observability MCP tools | Metrics, logs, traces correlation |
| GitHub MCP tools | Recent deploys, config changes, PRs |
| `kubectl get events` | Cluster-side timeline |
| Alert system queries | Firing/resolved alerts with timestamps |

---

### incident-historian

| Tool / Command | Purpose |
|----------------|---------|
| PagerDuty MCP tools | Alert/escalation history with timestamps |
| Slack MCP tools | Incident channel threads, decisions |
| GitHub MCP tools | Commits, PRs, deploys in incident window |

**Never runs:** Post to Slack, update PagerDuty, comment on GitHub

---

### postmortem-writer

| Tool / Command | Purpose |
|----------------|---------|
| File write (postmortem doc only) | Draft RCA document |
| No cluster/infra tools | Writes from validated evidence only |

---

### documentation-agent

| Tool / Command | Purpose |
|----------------|---------|
| `grep` / file search | Find all doc references to changed area |
| File read/write (isolated worktree) | Edit runbooks and docs |
| Git diff | Produce documentation change diff |

**Never runs:** `git push`, `git merge` — hands back diff for human review

---

## MCP Integrations Summary

| MCP Server | Used By | Purpose |
|------------|---------|---------|
| **Knowledge** | All agents | Persistent memory across sessions |
| **GitHub** | sre-investigator, incident-historian, gitops-reviewer | Commits, PRs, deploy history |
| **Slack** | incident-historian | Incident channel timeline |
| **PagerDuty** | incident-historian | Alert/escalation timestamps |
| **Argo CD** | gitops-reviewer | Application sync status, live state |
| **Observability** (Prometheus/Grafana/Datadog) | sre-investigator, deployment-verifier, finops-reviewer | Metrics, logs, traces |
| **Cost** (AWS Cost Explorer) | finops-reviewer | Spend attribution, usage data |

---

## Prohibited Commands (Never Executed)

These commands are explicitly forbidden across all agents:

| Category | Prohibited Commands |
|----------|-------------------|
| **Kubernetes writes** | `kubectl apply`, `kubectl delete`, `kubectl patch`, `kubectl scale`, `kubectl rollout restart`, `kubectl cordon`, `kubectl drain` |
| **Terraform writes** | `terraform apply`, `terraform destroy`, `terraform import` |
| **Helm writes** | `helm install`, `helm upgrade`, `helm rollback`, `helm uninstall` |
| **GitOps writes** | `argocd app sync`, `flux reconcile` |
| **etcd writes** | `etcdctl defrag`, `etcdctl snapshot restore`, `etcdctl member remove` |
| **Backup writes** | `velero backup create`, `velero restore create` |
| **Secret exposure** | `kubectl get secret -o jsonpath='{.data.*}'` (decoded values) |
