#!/bin/bash

# --- Configuration ---
# PLEASE FILL IN THESE VALUES BEFORE RUNNING THE SCRIPT
export YOUR_REGISTRY="your-docker-registry" # e.g., "docker.io/your-username" or your private registry
export AWS_ACCESS_KEY_ID="YOUR_AWS_ACCESS_KEY_ID"
export AWS_SECRET_ACCESS_KEY="YOUR_AWS_SECRET_ACCESS_KEY"
export S3_BUCKET_NAME="your-s3-bucket-name"

# --- Script Start ---
set -e # Exit immediately if a command exits with a non-zero status.

echo "🚀 Starting CloudIDE Deployment..."

# --- 1. Create Kubernetes Secrets ---
echo "🔑 Creating Kubernetes secret for AWS..."
kubectl create secret generic aws-secret \
  --from-literal=AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
  --from-literal=AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
  --from-literal=S3_BUCKET_NAME=${S3_BUCKET_NAME} \
  --dry-run=client -o yaml | kubectl apply -f -

echo "✅ AWS secret created."
echo "---"

# --- 2. Build and Push Docker Images ---
echo "🐳 Building and pushing Docker images..."

# Define services to build
services=("frontend" "orchestrator-simple" "runner" "init-service")

for service in "${services[@]}"; do
    IMAGE_NAME="${YOUR_REGISTRY}/cloudide-${service}:latest"
    echo "Building ${service} -> ${IMAGE_NAME}"
    docker build -t ${IMAGE_NAME} ./${service}
    echo "Pushing ${IMAGE_NAME}"
    docker push ${IMAGE_NAME}
    echo "✅ Image for ${service} pushed."
done

echo "✅ All Docker images have been built and pushed."
echo "---"


# --- 3. Update Kubernetes Deployment with New Images ---
echo "📝 Updating Kubernetes deployment file with your registry..."
# This command uses `sed` to replace the placeholder `cl0ud08` with your registry.
# It works on both macOS and Linux.
sed -i.bak "s|cl0ud08|${YOUR_REGISTRY}|g" k8s/deployment.yaml

echo "✅ k8s/deployment.yaml updated."
echo "---"


# --- 4. Deploy to Kubernetes ---
echo "🚢 Deploying all resources to Kubernetes..."
kubectl apply -f k8s/

echo "✅ All Kubernetes resources applied."
echo "---"


# --- 5. Final Instructions ---
echo "🎉 Deployment initiated successfully!"
echo ""
echo "To monitor the status, run the following commands:"
echo "  kubectl get pods -w"
echo "  kubectl get services"
echo ""
echo "Once the 'frontend-service' has an EXTERNAL-IP, you can access the application at that address."
echo "  kubectl get service frontend-service"
echo ""
