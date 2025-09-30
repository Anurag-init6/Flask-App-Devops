📦 Flask App DevOps Project

End-to-end DevOps demo with:

GitHub → Source Code Management

Jenkins → CI/CD Automation

Terraform → Infrastructure as Code

Ansible → Configuration Management

Kubernetes (Minikube) → Container Orchestration

AWS (EC2) → Cloud Infrastructure




🖥️ 1. Launch AWS EC2

Ubuntu 22.04 LTS, t2.medium

Open ports: 22, 80, 5000, 6443, 8080

SSH into server:

ssh -i mykey.pem ubuntu@<EC2_PUBLIC_IP>

⚙️ 2. Install Dependencies
sudo apt update && sudo apt upgrade -y

# Basic tools
sudo apt install -y git curl unzip wget apt-transport-https ca-certificates gnupg lsb-release

# Docker
sudo apt install -y docker.io
sudo usermod -aG docker $USER
newgrp docker

# Terraform
wget https://releases.hashicorp.com/terraform/1.9.5/terraform_1.9.5_linux_amd64.zip
unzip terraform_1.9.5_linux_amd64.zip
sudo mv terraform /usr/local/bin/

# Ansible
sudo apt install -y ansible

# Java for Jenkins
sudo apt install -y openjdk-11-jdk

📂 3. Clone Repository
git clone https://github.com/<your-username>/flask-app-devops.git
cd flask-app-devops

🐳 4. Build & Push Docker Image
cd app
docker build -t <dockerhub-username>/flask-app:latest .
docker login
docker push <dockerhub-username>/flask-app:latest
cd ..

☁️ 5. Provision Infra with Terraform
cd infra/terraform
terraform init
terraform apply -auto-approve
cd ../..

🔧 6. Configure Server with Ansible

Edit infra/ansible/hosts with your EC2 IP:

[servers]
<EC2_PUBLIC_IP> ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/mykey.pem


Run playbook:

cd infra/ansible
ansible-playbook -i hosts install_docker.yml
cd ../..

☸️ 7. Deploy on Kubernetes

Install kubectl & minikube:

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube


Start cluster:

minikube start --driver=docker


Apply manifests:

kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl get pods
kubectl get svc


Get service URL:

minikube service flask-service --url


Open in browser:

http://<MINIKUBE_URL>

🤖 8. Setup Jenkins (Optional CI/CD)
sudo apt install -y jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins


Access Jenkins:

http://<EC2_PUBLIC_IP>:8080


Use cicd/Jenkinsfile for pipeline config.

✅ Tech Stack

Python (Flask)

Docker

Jenkins

Terraform

Ansible

Kubernetes (Minikube)

AWS EC2
