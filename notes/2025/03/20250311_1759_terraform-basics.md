---
title: Terraform basics — working reference
slug: terraform-basics
tags: [terraform, devops, infrastructure]
---

# Terraform basics — working reference

Notes from getting up to speed on Terraform for the infrastructure work this quarter.

## Core concepts

**Provider:** Plugin that interfaces with an API (AWS, GCP, Azure, etc.). Declared in provider block.

**Resource:** Infrastructure object to create and manage.

**Data source:** Read existing infrastructure without managing it.

**State:** Terraform tracks what it manages in state files. The state is the source of truth for what Terraform knows about the real world.

**Module:** Reusable configuration package.

## Basic workflow

```bash
terraform init          # download providers, initialize backend
terraform plan          # show what would change (dry run)
terraform apply         # apply changes
terraform destroy       # destroy all managed resources
terraform show          # show current state
terraform state list    # list resources in state
terraform output        # show output values
```

## Simple resource example

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

variable "region" {
  default = "us-east-1"
}

resource "aws_s3_bucket" "logs" {
  bucket = "my-app-logs-${var.environment}"
}

output "bucket_arn" {
  value = aws_s3_bucket.logs.arn
}
```

## State management

- Use remote state (S3 + DynamoDB for locking on AWS) for any shared or production work
- Never commit state files to version control (they can contain secrets)
- Use `terraform import` to bring existing resources under management

Official documentation: [https://developer.hashicorp.com/terraform/docs](https://developer.hashicorp.com/terraform/docs)
