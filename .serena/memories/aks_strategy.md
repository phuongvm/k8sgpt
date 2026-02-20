# K8sGPT Strategy for AKS

## 1. Multi-Cluster Insights
Since you have multiple AKS clusters, you need a way to scan them all.

### Option A: CLI Automation (The "DevOps Job" Approach)
Write a script (Bash/Powershell) that loops through your Kubernetes contexts and runs `k8sgpt analyze`.
- **Prerequisite**: A `kubeconfig` file containing contexts for all your AKS clusters.
- **Command**: `k8sgpt analyze --context <context_name> --backend <backend> --no-cache --json > <context_name>_report.json`
- **Aggregation**: Collect these JSON reports and ingest them into a dashboard or simply store them in an Azure Storage Account.

### Option B: K8sGPT Operator
Install the K8sGPT Operator in **each** AKS cluster.
- **Pros**: Continuous monitoring inside the cluster.
- **Cons**: Decentralized. You'd need to check each cluster individually unless you export the results (e.g., to a centralized Prometheus/Grafana or a SIEM).

## 2. Integration & Sinks
- **Prometheus**: Configure the operator (or CLI in server mode) to export metrics to Prometheus. Use Grafana to visualize issue counts per cluster.
- **Slack/Teams**: Send critical alerts directly to your platform engineering channels.
- **Azure DevOps**: Run the CLI analysis as a pipeline step. Fail the build or create a bug item if critical issues are found.

## 3. Azure Specifics
- **Backend**: Use **Azure OpenAI** for the best compliance and performance on AKS.
- **Cache**: usage of Azure Blob Storage (`k8sgpt cache add azure ...`) is recommended to share analysis results across pipeline runs.
- **Control Plane**: Remember k8sgpt scans *resources* (Pods, Services). For AKS control plane issues (upgrades, quota, API server latency), continue using **Azure Monitor** and **AKS Diagnostics**. K8sGPT complements, but does not replace, Azure native tools.
