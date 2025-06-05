# GKE Simple Cluster with Two Workloads

This project sets up a GKE cluster with two node pools and deploys two applications:
1. A Tic Tac Toe web game
2. A Llama-based chat bot focused on Tic Tac Toe strategy

## Prerequisites

- Google Cloud Platform account
- gcloud CLI installed
- kubectl installed
- Docker installed
- Terraform installed

## Setup Instructions

1. Initialize Terraform:
```bash
terraform init
```

2. Create a terraform.tfvars file with your project ID:
```bash
echo 'project_id = "your-project-id"' > terraform.tfvars
```

3. Apply Terraform configuration:
```bash
terraform apply
```

4. Get credentials for the cluster:
```bash
gcloud container clusters get-credentials gke-cluster --region us-central1
```

5. Build and deploy the Tic Tac Toe application:
```bash
cd tic-tac-toe
docker build -t tic-tac-toe:latest .
kubectl apply -f k8s/deployment.yaml
```

6. Build and deploy the Llama chat application:
```bash
cd llama-chat
docker build -t llama-chat:latest .
kubectl apply -f k8s/deployment.yaml
```

## Accessing the Applications

1. Get the Tic Tac Toe service IP:
```bash
kubectl get service tic-tac-toe-service
```

2. Get the Llama chat service IP:
```bash
kubectl get service llama-chat-service
```

Access the applications using the external IPs provided by the services.

## Architecture

- The cluster has two node pools:
  - CPU pool: n1-standard-2 instances for the Tic Tac Toe game
  - GPU pool: n1-standard-2 instances with NVIDIA T4 GPUs for the Llama chat bot

- Both applications are exposed via LoadBalancer services
- The Tic Tac Toe game runs on the CPU pool
- The Llama chat bot runs on the GPU pool and is configured to use a single GPU

## Cleanup

To destroy all resources:
```bash
terraform destroy
```