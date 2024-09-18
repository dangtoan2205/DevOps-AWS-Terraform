## 1. Install Terraform

Link: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli

## 2. Quick start tutorial

Create a directory named `learn-terraform-docker-container`
----
```
mkdir learn-terraform-docker-container
```
Navigate into the working directory.
----
```
cd learn-terraform-docker-container
```
In the working directory, create a file called `main.tf` and paste the following Terraform configuration into it.
```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {
  host    = "npipe:////.//pipe//docker_engine"
}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}
```
Initialize the project, which downloads a plugin called a provider that lets Terraform interact with Docker.
----
```
terraform init
```
Provision the NGINX server container with `apply`. When Terraform asks you to confirm type `yes` and press `ENTER`.
----
```
terraform apply
```
Verify the existence of the NGINX container by visiting [localhost:8000](localhost:8000) in your web browser or running `docker ps` to see the container.
