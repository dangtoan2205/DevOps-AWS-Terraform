## 1. Tạo một file cấu hình có tên `main.tf`

### Chỉ định provider AWS
provider "aws" {
  region = "us-east-1" # Thay đổi theo vùng bạn muốn sử dụng
}

### Tạo một bucket S3
resource "aws_s3_bucket" "lession_bucket" {
  bucket = "lession" # Tên bucket S3, phải là duy nhất trong toàn cầu

### Thêm các tùy chọn khác nếu cần thiết
  acl    = "private" # Quyền truy cập mặc định

  tags = {
    Name = "Lession Bucket"
  }
}





# Chỉ định provider AWS
provider "aws" {
  region = "us-east-1" # Thay đổi theo vùng bạn muốn sử dụng
}

# Tạo một bucket S3
resource "aws_s3_bucket" "lession_bucket" {
  bucket = "lession" # Tên bucket S3, phải là duy nhất trong toàn cầu

  # Thêm các tùy chọn khác nếu cần thiết
  acl    = "private" # Quyền truy cập mặc định

  tags = {
    Name = "Lession Bucket"
  }
}

