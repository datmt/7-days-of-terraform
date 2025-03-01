# Terraform Cheat Sheet 🚀

## 1️⃣ Terraform Core Commands
```sh
# Initialize Terraform (downloads providers, sets up backend)
terraform init

# Show what Terraform will do before applying
terraform plan

# Apply the changes to the infrastructure
terraform apply

# Destroy all resources managed by Terraform
terraform destroy
```

## 2️⃣ Defining Resources & Providers
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

## 3️⃣ Managing State
```sh
# Show the current state
terraform state list

# View details of a resource in state
terraform state show aws_instance.example

# Import an existing resource into Terraform state
terraform import aws_instance.example i-1234567890abcdef0
```

## 4️⃣ Variables & Outputs
```hcl
# Define a variable with default value
variable "instance_type" {
  default = "t2.micro"
}

# Use the variable
resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = var.instance_type
}

# Define an output value
output "instance_ip" {
  value = aws_instance.example.public_ip
}
```

## 5️⃣ Using Modules for Reusability
```hcl
module "network" {
  source = "./modules/network"
  vpc_id = "vpc-123456"
}
```

## 6️⃣ Meta-Arguments: `count` & `for_each`
```hcl
# Create multiple instances dynamically
resource "aws_instance" "example" {
  count         = 3
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

```hcl
# Using for_each with maps
variable "users" {
  default = {
    user1 = "Admin"
    user2 = "Dev"
  }
}

resource "aws_iam_user" "example" {
  for_each = var.users
  name     = each.key
  tags     = { role = each.value }
}
```

## 7️⃣ Lifecycle Rules
```hcl
resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = "t2.micro"

  lifecycle {
    prevent_destroy = true  # Prevent accidental deletions
    ignore_changes  = [tags]  # Ignore external tag changes
  }
}
```

## 8️⃣ Best Practices ✅
- Always **use version control** (Git) for `.tf` files.
- Store state in **remote backends** (S3, Terraform Cloud) for team use.
- Always review `terraform plan` before applying.
- Use **modules** for reusability and maintainability.
- Keep **Terraform and providers updated**.

---
🚀 **Master these and you'll be 80% of the way to Terraform expertise!**

