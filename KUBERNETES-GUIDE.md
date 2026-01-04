# ⚡ Kubernetes & Docker DevOps - Complete Setup Guide

## Step 1: Prerequisites
```bash
# Check Docker
docker --version # >= 20.10

# Check kubectl
kubectl version # >= 1.24

# Check Helm
helm version # >= 3.0
```

## Step 2: Deploy on Kubernetes (Minikube/Local)

### Start Minikube
```bash
minikube start --memory=4096 --cpus=4
minikube addons enable ingress
```

### Deploy with Docker Compose for Quick Test
```bash
git clone https://github.com/Im044/kubernetes-docker-devops.git
cd kubernetes-docker-devops
docker-compose up -d
```

### Deploy on K8s with Helm
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-app bitnami/nginx --values values.yaml
kubectl get pods
kubectl port-forward svc/my-app 8080:80
```

## Step 3: Server Hardening (From server-hardening-security)

### Linux Server Hardening
```bash
cd ../server-hardening-security
# Run hardening scripts
bash linux-hardening.sh

# Verify CIS benchmarks
bash verify-cis.sh

# Configure firewall
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### Windows Server Hardening
```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
.\windows-hardening.ps1
.\disable-unnecessary-services.ps1
```

## Step 4: Azure Infrastructure (From azure-infrastructure-automation)

### Deploy to Azure
```bash
cd ../azure-infrastructure-automation

# Authenticate
az login
az account set --subscription "YOUR_SUBSCRIPTION_ID"

# Deploy Terraform
terraform init
terraform plan
terraform apply
```

### Verify Deployment
```bash
az group list
az vm list -d
az container registry list
```

## Integration: Full Stack

### 1. Deploy Docker Containers to AKS (Azure Kubernetes)
```bash
# Get AKS credentials
az aks get-credentials --resource-group myRG --name myAKS

# Deploy app
kubectl apply -f k8s-manifests/
```

### 2. Secure with Server Hardening
```bash
# Apply security policies
kubectl apply -f security-policies/

# Run security scanning
kubectl-score score k8s-manifests/
```

### 3. Monitor with Prometheus
```bash
helm install prometheus prometheus-community/kube-prometheus-stack
kubectl port-forward svc/prometheus-grafana 3000:80
# Access: http://localhost:3000 (admin/prom-operator)
```

## Testing & Validation

### Test Services
```bash
# Kubernetes health
kubectl get nodes
kubectl get pods --all-namespaces

# Port forwarding
kubectl port-forward svc/app 8080:80
curl http://localhost:8080

# Logs
kubectl logs -f deployment/app
```

### Performance Test
```bash
kubectl run -it --rm load-generator --image=busybox /bin/sh
# while true; do curl http://app:80; done
```

## Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Pod pending | `kubectl describe pod [pod-name]` - Check resources |
| CrashLoopBackOff | Check logs: `kubectl logs [pod-name]` |
| No Azure creds | Run `az login` and set subscription |
| Security denied | Apply NetworkPolicy: `kubectl apply -f network-policy.yaml` |

## All Repository Links

- 🐳 **Kubernetes**: https://github.com/Im044/kubernetes-docker-devops
- 🛡️ **Server Hardening**: https://github.com/Im044/server-hardening-security
- ☁️ **Azure Infrastructure**: https://github.com/Im044/azure-infrastructure-automation
