## 1. Tạo một file cấu hình có tên `main.tf`

## 2. Thêm nội dung vào file ``main.tf``
![image](https://github.com/user-attachments/assets/d2cbec07-33fe-4b1b-9831-061322c4a884)

#Chỉ định provider AWS </br>
provider "aws" { </br>
  region = "us-east-1" # Thay đổi theo vùng bạn muốn sử dụng </br>
} </br>

### Tạo một bucket S3
resource "aws_s3_bucket" "lession_bucket" {
  bucket = "lession" # Tên bucket S3, phải là duy nhất trong toàn cầu

#Thêm các tùy chọn khác nếu cần thiết
  acl    = "private" # Quyền truy cập mặc định

  tags = {
    Name = "Lession Bucket"
  }
}






