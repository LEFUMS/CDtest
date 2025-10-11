# Hello World Azure Kubernetes

This repository contains a simple "Hello World" Python application, ready to be deployed to an Azure Kubernetes Service (AKS) cluster.

The repository includes:
- A Python Flask application (`/app`)
- A `Dockerfile` for containerizing the application
- Kubernetes manifests for deployment and service (`/k8s`)
- A GitHub Actions workflow to build and push the Docker image to Azure Container Registry (ACR)

## Prerequisites

- An Azure account with an active subscription.
- An Azure Kubernetes Service (AKS) cluster.
- An Azure Container Registry (ACR).

## How to Use

### 1. Configure GitHub Secrets

This repository uses GitHub Actions to automate the process of building the Docker image and pushing it to your Azure Container Registry. To enable this, you need to configure the following secrets in your GitHub repository settings:

- `ACR_NAME`: The name of your Azure Container Registry.
- `ACR_USERNAME`: The username for your ACR.
- `ACR_PASSWORD`: The password for your ACR.

You can find these credentials in the Azure portal under your ACR's "Access keys" settings.

### 2. Run the Pipeline

Once the secrets are configured, the GitHub Actions workflow will automatically trigger on every push to the `main` branch. The workflow will:

1. Log in to your ACR.
2. Build the Docker image.
3. Push the image to your ACR with a tag corresponding to the commit SHA.
4. Update the `k8s/deployment.yaml` file with your ACR name and the new image tag.
5. Upload the updated Kubernetes manifests as a build artifact.

### 3. Deploy to AKS

After the pipeline has successfully run, you can deploy the application to your AKS cluster:

1. **Download the Kubernetes manifests** from the build artifacts of the latest workflow run.
2. **Connect to your AKS cluster** using `az aks get-credentials`.
3. **Apply the manifests** using `kubectl apply -f k8s/`.

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

### 4. Access the Application

Once the service is up and running, you can find the external IP address by running:

```bash
kubectl get svc hello-world-service
```

You can then access the application by navigating to `http://<EXTERNAL-IP>` in your web browser.