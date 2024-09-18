## 1. Define input variables
Link: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-variables

## 2. Prerequisites
After following the earlier tutorials, you will have a directory named `learn-terraform-aws-instance` with the following configuration in a file called `main.tf.` </br>
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
  ami           = "ami-08d70e59c07c61a3a"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```
![image](https://github.com/user-attachments/assets/75f87303-80a9-489b-b9dd-b0e1fc7ad8e3)

Ensure that your configuration matches this, and that you have run `terraform init` in the `learn-terraform-aws-instance` directory.

## 3. Set the instance name with a variable
The current configuration includes a number of hard-coded values. Terraform variables allow you to write configuration that is flexible and easier to re-use.

Add a variable to define the instance name.

Create a new file called `variables.tf` with a block defining a new `instance_name` variable.
```
variable "instance_name" {
  description = "Value of the Name tag for the EC2 instance"
  type        = string
  default     = "ExampleAppServerInstance"
}
```
![image](https://github.com/user-attachments/assets/74d986fd-a96d-4848-9b8b-6d59b677dc53)

In `main.tf`, update the `aws_instance` resource block to use the new variable. The `instance_name` variable block will default to its default value ("ExampleAppServerInstance") unless you declare a different value.
```
 resource "aws_instance" "app_server" {
   ami           = "ami-08d70e59c07c61a3a"
   instance_type = "t2.micro"

   tags = {
-    Name = "ExampleAppServerInstance"
+    Name = var.instance_name
   }
 }
```
![image](https://github.com/user-attachments/assets/a4051cd4-9a76-4379-ab6a-f457ea957537)

## 4. Apply your configuration
Apply the configuration. Respond to the confirmation prompt with a `yes`.
```
terraform apply
```
![image](https://github.com/user-attachments/assets/6bc80b04-5071-45e8-b0e5-381dbc35dcaa)

Now apply the configuration again, this time overriding the default instance name by passing in a variable using the `-var` flag. 
Terraform will update the instance's `Name` tag with the new name. Respond to the confirmation prompt with `yes`.

```
terraform apply -var "instance_name=YetAnotherName"
```
![image](https://github.com/user-attachments/assets/f2c6695e-f2ac-40a7-b130-df1499dd38e0)



























