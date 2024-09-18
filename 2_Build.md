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
![image](https://github.com/user-attachments/assets/2e29de0c-7f2d-41f5-85cd-ad4bfcf765dd)

## 4.Terraform Block
Khối này `terraform {}` chứa các thiết lập Terraform, bao gồm các nhà cung cấp bắt buộc mà Terraform sẽ sử dụng để cung cấp cơ sở hạ tầng của bạn. Đối với mỗi nhà cung cấp, thuộc sourcetính xác định tên máy chủ tùy chọn, không gian tên và loại nhà cung cấp. Terraform cài đặt các nhà cung cấp từ Terraform Registry theo mặc định. Trong cấu hình ví dụ này, `aws` nguồn của nhà cung cấp được xác định là `hashicorp/aws`, viết tắt của `registry.terraform.io/hashicorp/aws`.
Bạn cũng có thể đặt ràng buộc phiên bản cho mỗi nhà cung cấp được xác định trong `required_providers` khối. `version` Thuộc tính này là tùy chọn, nhưng chúng tôi khuyên bạn nên sử dụng nó để ràng buộc phiên bản nhà cung cấp để Terraform không cài đặt phiên bản nhà cung cấp không hoạt động với cấu hình của bạn. Nếu bạn không chỉ định phiên bản nhà cung cấp, Terraform sẽ tự động tải xuống phiên bản mới nhất trong quá trình khởi tạo.

## 5. Providers
Khối này `provider` cấu hình nhà cung cấp được chỉ định, trong trường hợp này là `aws`. Nhà cung cấp là một plugin mà Terraform sử dụng để tạo và quản lý tài nguyên của bạn.

Bạn có thể sử dụng nhiều khối nhà cung cấp trong cấu hình Terraform của mình để quản lý tài nguyên từ các nhà cung cấp khác nhau. Bạn thậm chí có thể sử dụng nhiều nhà cung cấp cùng nhau. Ví dụ: bạn có thể chuyển địa chỉ IP của phiên bản AWS EC2 của mình đến tài nguyên giám sát từ DataDog.

## 6. Resources
Sử dụng `resource` khối để xác định các thành phần của cơ sở hạ tầng của bạn. Một tài nguyên có thể là một thành phần vật lý hoặc ảo như phiên bản EC2 hoặc có thể là một tài nguyên logic như ứng dụng Heroku.

Khối tài nguyên có hai chuỗi trước khối: loại tài nguyên và tên tài nguyên. Trong ví dụ này, loại tài nguyên là aws_instancevà tên là app_server. Tiền tố của loại ánh xạ tới tên của nhà cung cấp. Trong cấu hình ví dụ, Terraform quản lý tài `aws_instance` nguyên với `aws` nhà cung cấp. Cùng nhau, loại tài nguyên và tên tài nguyên tạo thành một ID duy nhất cho tài nguyên. Ví dụ, ID cho phiên bản EC2 của bạn là `aws_instance.app_server`.

Khối tài nguyên chứa các đối số mà bạn sử dụng để cấu hình tài nguyên. Các đối số có thể bao gồm những thứ như kích thước máy, tên ảnh đĩa hoặc ID VPC. Tài liệu tham khảo nhà cung cấp của chúng tôi liệt kê các đối số bắt buộc và tùy chọn cho từng tài nguyên. Đối với phiên bản EC2 của bạn, cấu hình ví dụ đặt ID AMI thành ảnh Ubuntu và loại phiên bản thành `t2.micro`, đủ điều kiện cho tầng miễn phí của AWS. Nó cũng đặt một thẻ để đặt tên cho phiên bản.

## 7. Initialize the directory
Khi bạn tạo cấu hình mới — hoặc kiểm tra cấu hình hiện có từ kiểm soát phiên bản — bạn cần khởi tạo thư mục bằng `terraform init`.

Việc khởi tạo thư mục cấu hình sẽ tải xuống và cài đặt các nhà cung cấp được xác định trong cấu hình, trong trường hợp này là nhà cung cấp `aws`.

Khởi tạo thư mục.
![image](https://github.com/user-attachments/assets/8a473e8c-0d21-46b1-8b19-f6011b9e33e6)

Terraform tải xuống `aws` nhà cung cấp và cài đặt nó trong một thư mục con ẩn của thư mục làm việc hiện tại của bạn, có tên là `.terraform.` `terraform init` Lệnh in ra phiên bản nào của nhà cung cấp đã được cài đặt. Terraform cũng tạo một tệp khóa có tên `.terraform.lock.hcl` là chỉ định các phiên bản nhà cung cấp chính xác được sử dụng, để bạn có thể kiểm soát thời điểm bạn muốn cập nhật các nhà cung cấp được sử dụng cho dự án của mình.

## 9. Format and validate the configuration
We recommend using consistent formatting in all of your configuration files. The `terraform fmt` command automatically updates configurations in the current directory for readability and consistency.

Format your configuration. Terraform will print out the names of the files it modified, if any. In this case, your configuration file was already formatted correctly, so Terraform won't return any file names.
```
terraform fmt
```
You can also make sure your configuration is syntactically valid and internally consistent by using the `terraform validate` command.

Validate your configuration. The example configuration provided above is valid, so Terraform will return a success message.
```
terraform validate
```

## 10. Create infrastructure
Apply the configuration now with the `terraform apply` command. Terraform will print output similar to what is shown below. We have truncated some of the output to save space.
![image](https://github.com/user-attachments/assets/a4d39cae-a2f2-45fb-8484-d193fe1aa350)

Before it applies any changes, Terraform prints out the execution plan which describes the actions Terraform will take in order to change your infrastructure to match the configuration.
In this case the plan is acceptable, so type `yes` at the confirmation prompt to proceed. Executing the plan will take a few minutes since Terraform waits for the EC2 instance to become available
![image](https://github.com/user-attachments/assets/8325f8e4-e9b6-4f42-adfb-9f01f9cbeda0)




