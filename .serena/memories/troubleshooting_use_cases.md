# K8sGPT on AKS: Value and Use Cases

## What K8sGPT Does
- Runs built-in analyzers on cluster resources (Pods, Deployments, Nodes, PVCs, Services, Ingress, etc.) and flags likely issues (optional AI explanations).
- Exposes cluster data via MCP: list/get resources, logs, events, namespaces, cluster info, active filters.
- Provides guided workflows via prompts (troubleshoot-cluster, troubleshoot-pod, troubleshoot-deployment).

## Scenarios Where K8sGPT Is Most Valuable

1. **"Something is broken, I don't know where to start"** – Run `analyze` with `explain=true`; get a list of likely problem areas for triage.
2. **Pods not starting or crashing** – ImagePullBackOff, CrashLoopBackOff, OOMKilled, readiness/liveness, missing ConfigMaps/Secrets, PVC not bound; use get-resource + get-logs (previous=true) + list-events.
3. **Deployments stuck or not updating** – Replica mismatches, failed rollouts, ReplicaSet not progressing, HPA/PDB blocking.
4. **Storage / PVC issues** – PVC pending, PV Released, mount/permission issues (Azure Disk/File on AKS).
5. **Node and cluster capacity** – Node NotReady, pressure (memory/disk), resource exhaustion; list-resources (nodes) + analyze (Node filter).
6. **Networking / ingress** – Ingress/Service/HTTPRoute/Gateway misconfigs (e.g. no backend, wrong TLS, "no matches"); 502/503 triage.
7. **RBAC and security** – Quick Security/NetworkPolicy checks (supplement, not replace, Azure AD + RBAC review).
8. **Events and logs in one place** – list-events + get-logs for correlation (e.g. why pod was killed, namespace timeline).
9. **Consistent triage across clusters** – Same tools and prompts across dev/prod AKS contexts.
10. **AI-assisted explanation** – `explain=true` for short "why" and "what to check" for less experienced operators.

## Where K8sGPT Is Less Strong (AKS-Specific)
- **AKS control plane / API server** – No visibility into Azure-managed control plane → use Azure Portal, AKS diagnostics.
- **Azure networking (VNets, NSGs, private clusters)** – Cluster-internal view only → Azure Portal, az aks, Network Watcher.
- **Cost / SKU / scaling** – No cost or Azure scaling logic → Azure Cost Management, node pool/cluster autoscaler.
- **Azure integrations (Monitor, Container Insights, Managed Identity)** – Generic K8s view → Azure Monitor, Container Insights, Azure AD.
- **Deep app performance (latency, traces)** – No APM → Application Insights, Prometheus/Grafana (K8sGPT can use Prometheus integration for some metrics).

## Summary
K8sGPT is best for **workload and resource-level issues inside the cluster** (pods, deployments, storage, nodes, ingress, events, logs) and **fast, consistent triage** across AKS clusters. Pair with Azure tooling for control plane, networking, and cost.

## References
- K8sGPT MCP: `oss/k8sgpt/MCP.md`, `oss/k8sgpt/pkg/server/mcp.go`, `mcp_prompts.go`
- Setup: `tools/mcp-streamable-http/k8sgpt/setup_k8sgpt.cmd`
