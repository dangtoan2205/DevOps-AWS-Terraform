# Change infrastructure

## 1. Prerequisites
This tutorial assumes that you are continuing from the previous tutorials. If not, follow the steps below before continuing.

- Install the Terraform CLI (1.2.0+), and the AWS CLI (configured with a default profile), as described in the last tutorial.

- Create a directory named `learn-terraform-aws-instance` and paste the following configuration into a file named `main.tf`.
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-west-2"
}

resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```

![image](https://github.com/user-attachments/assets/ce46c218-ebc6-4ad4-81fb-cd12bd72b2aa)
- Initialize the configuration
```
terraform init
```
- Apply the configuration. Respond to the confirmation prompt with a yes.
```
terraform apply
```
Once you have successfully applied the configuration, you can continue with the rest of this tutorial.

## 2. Configuration
Now update the `ami` of your instance. Change the `aws_instance.app_server` resource under the provider block in `main.tf` by replacing the current AMI ID with a new one.</br>
![image](https://github.com/user-attachments/assets/a5a14f8e-cd70-47b4-a6a4-8acd6f1ed043)

This update changes the AMI to an Ubuntu 16.04 AMI. The AWS provider knows that it cannot change the AMI of an instance after it has been created, so Terraform will destroy the old instance and create a new one.

## 3. Apply Changes
After changing the configuration, run `terraform apply` again to see how Terraform will apply this change to the existing resources.</br>
![image](https://github.com/user-attachments/assets/69841e04-a065-4d0d-92e0-25f0aa23d1ee)
The prefix `-/+` means that Terraform will destroy and recreate the resource, rather than updating it in-place. Terraform can update some attributes in-place (indicated with the ~ prefix), but changing the AMI for an EC2 instance requires recreating it. Terraform handles these details for you, and the execution plan displays what Terraform will do.

Additionally, the execution plan shows that the AMI change is what forces Terraform to replace the instance. Using this information, you can adjust your changes to avoid destructive updates if necessary.

Once again, Terraform prompts for approval of the execution plan before proceeding. Answer yes to execute the planned steps.</br>

![image](https://github.com/user-attachments/assets/a6922801-5993-4360-891b-35666e54759c)

As indicated by the execution plan, Terraform first destroyed the existing instance and then created a new one in its place. You can use `terraform show` again to have Terraform print out the new values associated with this instance.














