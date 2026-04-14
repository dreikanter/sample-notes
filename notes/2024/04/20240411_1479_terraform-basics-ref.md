# Terraform basics reference

Working through Terraform for the first time on a real project. Notes on the concepts that aren't obvious from examples.

## State

Terraform maintains a state file (`terraform.tfstate`) that maps your configuration to real infrastructure. This is what allows `plan` to show diffs.

**Key operations:**
```bash
terraform init        # initialize, download providers
terraform plan        # show what would change
terraform apply       # apply changes
terraform destroy     # tear down everything
terraform show        # human-readable state
terraform state list  # all resources in state
terraform state rm <resource>   # remove from state without destroying
terraform import <resource> <id>  # import existing infra into state
```

## Resource syntax

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-unique-bucket-name"
  tags = {
    Environment = "prod"
    Team        = "platform"
  }
}

# Reference: aws_s3_bucket.my_bucket.id
```

## Variables

```hcl
variable "region" {
  type        = string
  default     = "us-east-1"
  description = "AWS region"
}

# Use: var.region
```

Override: `terraform apply -var="region=us-west-2"` or `terraform.tfvars` file.

## Outputs

```hcl
output "bucket_arn" {
  value = aws_s3_bucket.my_bucket.arn
}
```

## Data sources (read existing infra)

```hcl
data "aws_vpc" "main" {
  tags = { Name = "main-vpc" }
}

# Reference: data.aws_vpc.main.id
```

## Modules

```hcl
module "networking" {
  source  = "./modules/networking"
  vpc_cidr = "10.0.0.0/16"
}
```

## Remote state

```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
    dynamodb_table = "terraform-locks"
  }
}
```

The DynamoDB table provides state locking — prevents concurrent applies.

Docs: https://developer.hashicorp.com/terraform/docs
