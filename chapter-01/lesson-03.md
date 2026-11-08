# Lesson 3: Real-world use cases of Sveltos

Let's understand where companies actually benefit from Sveltos.

## 1. Managing add-ons across multiple environments

Imagine a company has three environments:
- Development
- Staging
- Production
  
Each environment has its own Kubernetes cluster.

The company wants to install these tools:

| Add-on          | Development | Staging | Production |
|-----------------|-------------|---------|------------|
| Kyverno         | Yes         | Yes     | Yes        |
| Prometheus      | Yes         | Yes     | Yes        |
| Grafana         | Yes         | Yes     | Yes        |
| cert-manager    | No          | Yes     | Yes        |
| Debugging tools | Yes         | Yes     | No         |

- Without a central management solution, engineers might install and update these tools separately in every cluster.
- With Sveltos, you can define the desired configuration and use cluster labels to select the target clusters.

For example:
```yaml
clusterSelector:
  matchLabels:
    environment: production
```
This selects clusters labelled `environment=production`.

### Why is this useful?
You can maintain consistent configurations across environments without manually repeating the same installation work.

---

## 2. Managing security policies across a fleet

Imagine your company has **_30 Kubernetes clusters_**.

Your security team wants to enforce these rules:
- Containers must not run as root.
- Images must come from approved registries.
- Containers must not use privileged mode.
- Every application must have resource requests and limits.
  
i) You can use **_Kyverno_** to implement these policies.

ii) Sveltos can distribute Kyverno and its policy resources to selected clusters.

For example:

```text
Management cluster
       |
       | Sveltos
       |
       +---- Dev cluster
       |       └── Kyverno + policies
       |
       +---- Staging cluster
       |       └── Kyverno + policies
       |
       +---- Production cluster
               └── Kyverno + policies
```

Now your company can manage security policies centrally.

One important distinction:
- Sveltos distributes and manages the Kyverno installation and policy resources.
- Kyverno evaluates Kubernetes admission requests and enforces the policies.

Sveltos and Kyverno have different responsibilities.

---

## 3. Standardizing monitoring across clusters
Suppose a company operates 50 clusters across AWS and GCP.

The operations team wants every cluster to have:
- Prometheus for metrics collection.
- Grafana for dashboards.
- Alertmanager for alert handling.

1) Instead of manually installing these tools 50 times, Sveltos can deploy the monitoring stack across the selected clusters.
2) If you need to upgrade the monitoring chart version, you can update the desired configuration and let Sveltos reconcile the selected clusters.

---

## 4. Managing clusters across multiple cloud providers

Suppose your organization has:
- 5 EKS clusters in AWS.
- 5 GKE clusters in Google Cloud.
- 3 on-premises Kubernetes clusters.
  
1) Sveltos can manage these clusters together, provided they are registered and reachable using a supported deployment model.

2) It doesn't matter that the clusters were created using different infrastructure providers.

For example:

| Cluster	       | Platform	  | Environment   |
|----------------|------------|---------------|
| cluster-aws-1	 | EKS	       | Production    |
| cluster-aws-2	 | EKS	       | Development   |
| cluster-gcp-1	 | GKE	       | Production    |
| cluster-onprem-1	 | On-premises | Production    |

You can use labels to select clusters based on:
- Cloud provider.
- Environment.
- Region.
- Business unit.
- Application type.

For example, you might deploy a particular add-on only to clusters labeled `cloud=aws`.
This is one of the most useful capabilities for multi-cloud Kubernetes operations.

---

## 5. Managing applications at remote edge locations
Imagine a retail company has 100 stores.

Every store has a small Kubernetes cluster running:
- Inventory services.
- Local APIs.
- Monitoring agents.
- Store-management applications.
  
i) The company wants to distribute configuration and application updates to all stores.

ii) However, some stores have limited internet connectivity and strict firewall rules.

iii) Sveltos can help distribute the required configuration across these clusters. Pull Mode can be useful when the stores cannot accept incoming connections from the central management environment.

iv) This is where our future **Sveltos for Edge Kubernetes** lesson will be especially useful.

---

## 6. Managing application dependencies

Suppose an application needs these components:
1. A namespace.
2. A database.
3. A backend application.
4. A frontend application.
5. An Ingress controller.
   
You cannot always deploy everything in an arbitrary order.

For example, the backend may need the database to be ready before it starts.

- Sveltos supports dependencies and ordering mechanisms so that you can express relationships between configurations.

---

## 7. Where does Sveltos fit with other Kubernetes tools?

You will encounter many tools in real Kubernetes environments. Let's understand their main responsibilities.

| Tool        | Main responsibility                                            |
|-------------|----------------------------------------------------------------|
| Cluster API | 	Creates and manages Kubernetes clusters                       |
| Sveltos     | 	Distributes and manages add-ons and resources across clusters |
| Argo CD     | 	Synchronizes applications from Git repositories               |
| Flux        | 	Provides GitOps-based delivery and reconciliation             |
| Kustomize   | 	Customizes Kubernetes YAML manifests                          |
| Helm        | 	Packages and installs Kubernetes applications                 |
| Kyverno     | 	Enforces Kubernetes policies                                  |
| Prometheus  | 	Collects and stores metrics                                   |
| Grafana     | 	Displays dashboards and visualizes data                       |

These tools can work together.

For example:

```text
Cluster API
    |
    | Creates Kubernetes clusters
    v
Kubernetes clusters
    |
    | Sveltos manages add-ons
    v
Argo CD + Kyverno + Prometheus
    |
    | Argo CD synchronizes application configuration
    v
Your application
```

Remember these distinctions:
- Sveltos does not replace Helm. It can use Helm to deploy charts.
- Sveltos does not replace Kustomize. It can use Kustomize to customize manifests.
- Sveltos does not replace Kyverno. It can distribute Kyverno.
- Sveltos does not replace Cluster API. They solve different primary problems.

---
