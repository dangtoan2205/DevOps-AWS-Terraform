## 1. Query data with outputs
Link: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-outputs

## 2. Initial configuration
After following the previous tutorials, you will have a directory named `learn-terraform-aws-instance` with the following configuration.

```
# main.tf

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
  ami           = "ami-08d70e59c07c61a3a"
  instance_type = "t2.micro"

  tags = {
    Name = var.instance_name
  }
}

# variables.tf

variable "instance_name" {
  description = "Value of the Name tag for the EC2 instance"
  type        = string
  default     = "ExampleAppServerInstance"
}
```
![image](https://github.com/user-attachments/assets/61e8fcd6-0878-4ca7-a3e2-015e94f09470)

Ensure that your configuration matches this, and that you have initialized your configuration in the `learn-terraform-aws-instance` directory.

```
terraform init
```

Apply the configuration before continuing this tutorial. Respond to the confirmation prompt with a `yes`.

```
terraform apply
```

## 3. Output EC2 instance configuration
Create a file called `outputs.tf` in your `learn-terraform-aws-instance` directory.

Add the configuration below to `outputs.tf` to define outputs for your EC2 instance's ID and IP address.
```
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.app_server.id
}

output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.app_server.public_ip
}
```
![image](https://github.com/user-attachments/assets/f6e1fe73-a59c-4be5-abe6-00dcce278e18)

## 4. Inspect output values
You must apply this configuration before you can use these output values. Apply your configuration now. Respond to the confirmation prompt with `yes`.
```
terraform apply
```
![image](https://github.com/user-attachments/assets/a99f4abc-95fd-4954-b997-14237f08f0a3)

Terraform prints output values to the screen when you apply your configuration. Query the outputs with the `terraform output` command.
```
terraform output
```
![image](https://github.com/user-attachments/assets/f596bcd9-bffc-4b8c-890c-4c32632760c1)

You can use Terraform outputs to connect your Terraform projects with other parts of your infrastructure, or with other Terraform projects. To learn more, follow our in-depth tutorial, `Output Data from Terraform`

## 5. Destroy infrastructure
Destroy your infrastructure. Respond to the confirmation prompt with `yes`.
```
terraform destroy
```
![image](https://github.com/user-attachments/assets/37ff8145-c302-4e52-8086-b07b1eaff832)

























