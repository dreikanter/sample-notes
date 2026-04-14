---
title: Terraform basics — working notes
slug: terraform-basics
tags: [terraform, infrastructure, devops]
description: Practical notes on Terraform for managing cloud infrastructure.
---

# Terraform basics — working notes

Learning Terraform for managing AWS infrastructure. The [Terraform documentation](https://developer.hashicorp.com/terraform/docs) is dense but good; these are my working notes for things I keep needing to look up.

## Core commands

```bash
terraform init          # Download providers, set up backend
terraform plan          # Preview changes (always run first)
terraform apply         # Apply changes (prompts for confirmation)
terraform apply -auto-approve  # Apply without prompt (CI only)
terraform destroy       # Tear down everything
terraform output        # Show outputs
terraform state list    # List all resources in state
terraform import <resource> <id>  # Import existing resource to state
```

## Basic resource structure

```hcl
resource "aws_s3_bucket" "logs" {
  bucket = "my-app-logs-${var.environment}"
  
  tags = {
    Environment = var.environment
    ManagedBy   = "terraform"
  }
}

variable "environment" {
  type    = string
  default = "development"
}

output "bucket_arn" {
  value = aws_s3_bucket.logs.arn
}
```

## State management

State is how Terraform knows what exists. In a team:
- Use remote state (S3 + DynamoDB for AWS)
- Never edit state manually — use `terraform state` commands
- State can contain secrets — treat it as sensitive

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "eu-west-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

## Modules

Modules are reusable configurations. Use the [Terraform Registry](https://registry.terraform.io) for community modules, or write your own for internal standards.

## Gotchas

- `terraform apply` can delete and recreate resources — always read the plan carefully, especially for stateful resources like databases
- Some changes force replacement even when you only wanted an update (e.g., changing the name of an S3 bucket)
- `depends_on` is occasionally necessary but often a signal you're missing an implicit reference
