# LAB: Bạn là người có quyền admin trên AWS. Dự án X cần cấp quyền cho user A có thể upload file vào bucket lession1, user B chỉ có quyền download file từ bucket lession1.
----

`main.tf`
```
provider "aws" {
  region = "us-west-2"  # Thay doi vung
}

# Tao IAM Policy
resource "aws_iam_policy" "custom_policy" {
  name        = "CustomPolicy"
  description = "Custom IAM policy with S3 and STS permissions."

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Sid    = "AllowAssumeRole"
        Effect = "Allow"
        Action = "sts:AssumeRole"
        Resource = "arn:aws:iam::009160048703:role/S3DemoAssumeRole"
      },
      {
        Sid    = "SeeAllBucketAvaliableBuckets123"
        Effect = "Allow"
        Action = "s3:ListAllMyBuckets"
        Resource = "*"
      },
      {
        Sid    = "SeeAllBucketAvaliableBuckets"
        Effect = "Allow"
        Action = [
          "s3:ListBucket",
          "s3:PutObject"
        ]
        Resource = [
          "arn:aws:s3:::mylession01",
          "arn:aws:s3:::mylession01/*"
        ]
      }
    ]
  })
}

# Tao S3 Bucket
resource "aws_s3_bucket" "mylession01" {
  bucket = "mylession01"
  acl    = "private"
}

# Tao IAM Role cho phep userA gia dinh
resource "aws_iam_role" "s3_demo_role" {
  name = "S3DemoAssumeRole"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect = "Allow",
        Principal = {
          AWS = "arn:aws:iam::009160048703:user/userA"
        },
        Action = "sts:AssumeRole",
        Condition = {}
      }
    ]
  })
}

# Tao IAM User A
resource "aws_iam_user" "userA" {
  name = "userA"
}

# Tao IAM User B
resource "aws_iam_user" "userB" {
  name = "userB"
}

# Gan IAM Policy cho User A
resource "aws_iam_policy_attachment" "userA_policy" {
  name       = "userA-attachment"
  users      = [aws_iam_user.userA.name]
  policy_arn = aws_iam_policy.custom_policy.arn
}

# Gan IAM Policy cho User B
resource "aws_iam_policy_attachment" "userB_policy" {
  name       = "userB-attachment"
  users      = [aws_iam_user.userB.name]
  policy_arn = aws_iam_policy.custom_policy.arn
}
```

-----
## 1. Provider
```
provider "aws" {
  region = "us-west-2"
}
```
- Provider: Xác định AWS là nhà cung cấp dịch vụ đám mây và chọn khu vực us-west-2.

## 2. IAM Policy
```
resource "aws_iam_policy" "custom_policy" {
  name        = "CustomPolicy"
  description = "Custom IAM policy with S3 and STS permissions."

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Sid    = "AllowAssumeRole"
        Effect = "Allow"
        Action = "sts:AssumeRole"
        Resource = "arn:aws:iam::009160048703:role/S3DemoAssumeRole"
      },
      {
        Sid    = "SeeAllBucketAvaliableBuckets123"
        Effect = "Allow"
        Action = "s3:ListAllMyBuckets"
        Resource = "*"
      },
      {
        Sid    = "SeeAllBucketAvaliableBuckets"
        Effect = "Allow"
        Action = [
          "s3:ListBucket",
          "s3:PutObject"
        ]
        Resource = [
          "arn:aws:s3:::mylession01",
          "arn:aws:s3:::mylession01/*"
        ]
      }
    ]
  })
}
```
- IAM Policy: Tạo một chính sách IAM cho phép:
  - Người dùng có thể giả định vai trò S3DemoAssumeRole.
  - Danh sách tất cả các bucket S3.
  - Thực hiện các hành động ListBucket và PutObject trên bucket mylession01.

## 3. S3 Bucket
```
resource "aws_s3_bucket" "mylession01" {
  bucket = "mylession01"
  acl    = "private"
}
```
- S3 Bucket: Tạo một bucket S3 với tên mylession01 và quyền truy cập riêng tư.

## 4. IAM Role
```
resource "aws_iam_role" "s3_demo_role" {
  name = "S3DemoAssumeRole"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect = "Allow",
        Principal = {
          AWS = "arn:aws:iam::009160048703:user/userA"
        },
        Action = "sts:AssumeRole",
        Condition = {}
      }
    ]
  })
}
```
- IAM Role: Tạo một vai trò IAM cho phép người dùng userA giả định vai trò này. Vai trò này được sử dụng để cấp quyền cho các hành động liên quan đến S3.

## 5. IAM Users
```
resource "aws_iam_user" "userA" {
  name = "userA"
}

resource "aws_iam_user" "userB" {
  name = "userB"
}
```
- IAM Users: Tạo hai người dùng IAM là userA và userB.

## 6. Gán Policy cho Users
```
resource "aws_iam_policy_attachment" "userA_policy" {
  name       = "userA-attachment"
  users      = [aws_iam_user.userA.name]
  policy_arn = aws_iam_policy.custom_policy.arn
}

resource "aws_iam_policy_attachment" "userB_policy" {
  name       = "userB-attachment"
  users      = [aws_iam_user.userB.name]
  policy_arn = aws_iam_policy.custom_policy.arn
}
```
- Policy Attachment: Gán chính sách custom_policy cho cả hai người dùng userA và userB































