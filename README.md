Terraform + Docker: Deploy NGINX

This repository demonstrates how to use Terraform with the Docker provider to deploy an **NGINX container** locally.

Step 1: Install Prerequisites
-->Update Your System
sudo apt update && sudo apt upgrade -y

-->Install Docker
sudo apt install docker.io -y
sudo systemctl enable --now docker
sudo usermod -aG docker $USER

-->Verify Docker
docker --version
docker run hello-world

-->Install Terraform
sudo apt install wget unzip -y
wget https://releases.hashicorp.com/terraform/1.6.5/terraform_1.6.5_linux_amd64.zip
unzip terraform_1.6.5_linux_amd64.zip
sudo mv terraform /usr/local/bin/
terraform -version


Step 2: Create Working Directory
mkdir ~/terraform-docker
cd ~/terraform-docker


Step 3: Terraform Configuration
vi.main.tf

# Specify Terraform version
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0"
    }
  }
}


provider "docker" {}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
resource "docker_container" "nginx_container" {
  name  = "nginx-container"
  image = docker_image.nginx.name
  ports {
    internal = 80
    external = 8080
  }
}



Step 4: Initialize Terraform
terraform init

Step 5: Preview Changes
terraform plan

Step 6: Apply Configuration
terraform apply

Step 7: Verify Deployment
docker ps

Step 8: Check Terraform State
terraform state list
terraform show

Step 9:Destroy Infrastructure
terraform destroy
