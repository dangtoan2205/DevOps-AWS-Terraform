## 1.Build infrastructure

## 2. Prerequisites
To use your IAM credentials to authenticate the Terraform AWS provider, set the `AWS_ACCESS_KEY_ID` environment variable.
```
aws configure
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: Enter
Default output format [None]: Enter
```
### 3. Write configuration
The set of files used to describe infrastructure in Terraform is known as a Terraform configuration. You will write your first configuration to define a single AWS EC2 instance.
</br>
Each Terraform configuration must be in its own working directory. Create a directory for your configuration.
```
mkdir learn-terraform-aws-instance
```
Change into the directory.
```
cd learn-terraform-aws-instance
```
Create a file to define your infrastructure.
```
touch main.tf
```
Open `main.tf` in your text editor, paste in the configuration below, and save the file.
```
code main.tf
```
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
This is a complete configuration that you can deploy with Terraform. The following sections review each block of this configuration in more detail.
## 4.Terraform Block


















