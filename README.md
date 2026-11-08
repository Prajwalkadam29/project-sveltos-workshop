# Learning Sveltos

## What is Project Sveltos?
Project Sveltos is an open-source Kubernetes add-on management system designed to deploy and manage Kubernetes add-ons and applications **across multiple Kubernetes clusters.**

Let's understand every part of this definition.

### 1. What is a Kubernetes add-on?
Before understanding Sveltos, we need to understand what Kubernetes add-ons are.

Imagine that you create a Kubernetes cluster.

Initially, you have the Kubernetes control plane and worker nodes. You can deploy applications using Kubernetes resources such as:
- Deployments
- Services
- ConfigMaps
- Secrets
- StatefulSets
- Ingresses

But a production Kubernetes cluster generally needs much more than application workloads.

For example:

| Requirement	           | Example software          |
|------------------------|---------------------------|
| Networking	           | Cilium, Calico            |
| Monitoring	           | Prometheus                |
| Visualization	         | Grafana                   |
| Logging	               | Fluent Bit                |
| Ingress management	   | ingress-nginx             |
| Certificate management |	cert-manager             |
| DNS management	       | ExternalDNS               |
| Secrets management	   | External Secrets Operator |
| Storage	               | Rook                      |
| Autoscaling	           | Cluster Autoscaler        |

These additional components extend the functionality of Kubernetes.

We commonly call them **Kubernetes add-ons.**

### Important terminology

Let's distinguish three terms:

**1. Kubernetes core components**
These are components required to operate Kubernetes itself, such as the API server, scheduler and controller manager.

**2. Kubernetes add-ons**
These provide additional cluster capabilities, such as networking, monitoring, logging and ingress management.

**3. Applications**
These are the actual workloads that your organization wants to run, such as frontend services, backend APIs and databases.

Sveltos can manage both add-ons and applications.
For example, we could use Sveltos to deploy Prometheus and Grafana to ten Kubernetes clusters, while also deploying a specific application only to production clusters.

This brings us to the problem Sveltos addresses.

---

### 2. Why do we need add-on management?

Let's consider a real-world situation.

Imagine you are a platform engineer in an organization.

Your organization has 20 Kubernetes clusters:
- 5 development clusters.
- 5 staging clusters.
- 5 production clusters.
- 5 disaster-recovery clusters.

Each cluster requires:
- Prometheus.
- Grafana.
- cert-manager.
- An ingress controller.
- A logging agent.
- Security policies.

Without a centralized add-on management solution, you might manage these components separately.
