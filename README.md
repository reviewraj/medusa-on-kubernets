
# Medusa on Kubernetes with Helm Charts

This repository contains Helm charts for deploying a Medusa server along with a PostgreSQL database on a Kubernetes cluster.

## Prerequisites

Before using these Helm charts, ensure that you have the following tools installed and configured:

- [Helm](https://helm.sh/docs/intro/install/) (version 3+)
- [Kubernetes](https://kubernetes.io/docs/tasks/tools/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) configured to access your Kubernetes cluster

## Deployment Overview

The Helm chart in this repository provides the setup for running Medusa, a headless commerce platform, alongside a PostgreSQL database.

### What's Included

- **Medusa Server**: The Medusa server runs on port `9000`.
- **PostgreSQL**: A PostgreSQL container for managing Medusa's database needs.

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/reviewraj/medusa-on-kubernets.git
cd medusa-on-kubernets
```

### 2. Install the Helm Chart

You can install the Helm chart from the packaged `.tgz` file. This chart will deploy both Medusa and PostgreSQL on your Kubernetes cluster.

```bash
helm install medusa ./medusa-chart.tgz
```

By default, the Helm chart will deploy the Medusa server with the default configuration. If you wish to customize the deployment (e.g., configure database credentials, change resource limits), you can create and pass a custom `values.yml` file.

```bash
helm install medusa ./medusa-chart.tgz --values custom-values.yml
```

### 3. Verify the Deployment

Once the installation is complete, you can verify that the Medusa server and PostgreSQL are running successfully.

```bash
kubectl get pods
kubectl get svc
```

Look for the service `first-release-medusa-medusa-container` and ensure that it is running. If using a NodePort, you can access the Medusa server via the external IP or via port-forwarding.

```bash
kubectl port-forward svc/first-release-medusa-medusa-container 9000:9000
```

### 4. Accessing the Application

Once deployed, you can access the Medusa server by navigating to the appropriate IP address and port, or use port-forwarding to access it locally:

```bash
curl http://localhost:9000
```

## Customizing the Helm Chart

To customize the Medusa or PostgreSQL configuration, modify the `values.yml` file or pass custom values when installing the Helm chart.

### Example `values.yml`:

```yaml
medusa:
  image:
    repository: medusajs/medusa
    tag: latest
  service:
    type: NodePort
    port: 9000

postgres:
  image:
    repository: postgres
    tag: latest
  service:
    type: NodePort
    port: 5432
```

## Uninstalling the Helm Chart

If you need to remove the deployment, use the following command:

```bash
helm uninstall medusa
```

This will remove all resources created by the Helm chart.

## Conclusion

This Helm chart provides a simple and scalable way to deploy Medusa on Kubernetes. Feel free to contribute and modify the chart as needed for your use case.

## License

This project is licensed under the MIT License.
