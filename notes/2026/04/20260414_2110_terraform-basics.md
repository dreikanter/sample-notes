---
title: Terraform basics reference
slug: terraform-basics
tags: [terraform, infra, dev, cheatsheet]
description: Core Terraform concepts and commands for someone coming back to it after a while
---

# Terraform basics reference

Coming back to Terraform after several months. Notes for re-orientation.

## Core concepts

**Provider**: plugin that talks to an API (AWS, GCP, Cloudflare, etc.)
**Resource**: infrastructure object managed by Terraform
**Data source**: read existing infrastructure not managed by Terraform
**State**: Terraform's record of what it has created

## Basic workflow

```bash
terraform init       # download providers, initialize backend
terraform plan       # show changes that would be made
terraform apply      # apply changes (prompts for confirmation)
terraform destroy    # destroy all managed resources
```

```bash
terraform plan -out=tfplan          # save plan to file
terraform apply tfplan              # apply saved plan (no prompt)
```

## State management

```bash
terraform state list                # list all resources in state
terraform state show aws_instance.web  # show specific resource
terraform import aws_instance.web i-1234567890  # import existing resource
terraform state rm aws_instance.web    # remove from state without deleting
```

## Workspaces

```bash
terraform workspace list
terraform workspace new staging
terraform workspace select staging
```

Use `terraform.workspace` variable to vary config by environment.

## Variable patterns

```hcl
variable "environment" {
  type        = string
  description = "Deployment environment"
  default     = "dev"
}

# Use it
resource "aws_instance" "web" {
  tags = {
    Environment = var.environment
  }
}
```

Override: `terraform apply -var="environment=prod"` or via `terraform.tfvars` file.

## Locals

```hcl
locals {
  common_tags = {
    Project     = "my-app"
    Environment = var.environment
    ManagedBy   = "terraform"
  }
}
```

Reference: https://developer.hashicorp.com/terraform/docs
Registry: https://registry.terraform.io
