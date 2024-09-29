# Medusa on Kubernetes

This project demonstrates the deployment of Medusa, an open-source headless commerce engine, on a Kubernetes cluster using Helm charts. The setup includes two separate containers for the **Medusa** server and **PostgreSQL** database, both running as independent services but configured to work together.

## Project Structure

- **medusa-container**: This container runs the Medusa server.
- **postgres-container**: This container runs PostgreSQL, which is required for Medusa's database operations.

Both containers are managed separately in Kubernetes, allowing for independent scaling and configuration.

### Key Components:

- **Helm Charts**: Used to manage both the Medusa and PostgreSQL deployments, including service discovery, persistent volumes, and container lifecycle management.
- **NodePort Services**: Configured to expose both containers via NodePort for external access.
- **Persistent Volumes**: Ensures that PostgreSQL data is persisted across container restarts.

## Prerequisites

Before running the Medusa and Postgres containers on Kubernetes, ensure you have:

- A Kubernetes cluster running (e.g., local Kind cluster or cloud-based Kubernetes service).
- Helm installed and configured in your environment.
- Docker images for Medusa and Postgres available (pulled from Docker Hub or built locally).

## Deployment

### Steps to Deploy

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/reviewraj/medusa-on-kubernets.git
   cd medusa-on-kubernets

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
