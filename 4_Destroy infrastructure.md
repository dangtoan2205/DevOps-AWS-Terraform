## 1. Destroy infrastructure
Link: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-destroy

## 2. Destroy
The `terraform destroy` command terminates resources managed by your Terraform project. This command is the inverse of `terraform apply` in that it terminates all the resources specified in your Terraform state. 
It does not destroy resources running elsewhere that are not managed by the current Terraform project.

Destroy the resources you created.</br>
![image](https://github.com/user-attachments/assets/acd8a0b8-314c-408c-9a28-17491f522f71)

The `-` prefix indicates that the instance will be destroyed. As with apply, Terraform shows its execution plan and waits for approval before making any changes.

Answer `yes` to execute this plan and destroy the infrastructure.</br>
![image](https://github.com/user-attachments/assets/8f4bf5bf-2d88-4761-a5b1-f7f5def9b949)

Just like with `apply`, Terraform determines the order to destroy your resources. In this case, Terraform identified a single instance with no other dependencies, so it destroyed the instance. 
In more complicated cases with multiple resources, Terraform will destroy them in a suitable order to respect dependencies.
























