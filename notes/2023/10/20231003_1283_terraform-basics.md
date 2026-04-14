---
title: Terraform — getting oriented after years of avoiding it
slug: terraform-basics
tags: [terraform, infrastructure, devops]
description: First practical Terraform notes as I start learning it for the migration project.
---

# Terraform — getting oriented after years of avoiding it

Context: the EKS migration pilot [[20230909_1259]] means I need to understand Terraform since the infra team uses it for all AWS resources.

Official docs: https://developer.hashicorp.com/terraform/docs

## Core concepts

**Provider:** Plugin that talks to an API (AWS, GCP, Azure, etc.). Configuration:

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
  region = "us-east-1"
}
```

**Resource:** An infrastructure object.

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-project-data-2023"
}
```

**Data source:** Read-only query of existing infrastructure.

```hcl
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]  # Canonical
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-*-22.04-amd64-server-*"]
  }
}
```

**State:** Terraform tracks what it has created in a state file. Remote state in S3 with DynamoDB locking is the standard production pattern.

## Workflow

```bash
terraform init     # download providers
terraform plan     # show what will change
terraform apply    # apply changes
terraform destroy  # remove everything
```

Always `plan` before `apply`. The plan output is readable and worth reviewing carefully before any apply in production.

## Modules

Reusable, composable groups of resources. The infra team has modules for VPC, EKS cluster, and RDS. I'll use those rather than writing from scratch.

```hcl
module "vpc" {
  source = "./modules/vpc"
  cidr   = "10.0.0.0/16"
}
```
