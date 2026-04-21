# ROSA HCP on AWS with Terraform

This repository provisions a Red Hat OpenShift Service on AWS, using the Hosted Control Plane deployment model and Terraform.

Overview

This configuration can do two things.

1.Create a new AWS VPC for the ROSA cluster.
2.Deploy the ROSA cluster into existing AWS subnets.

What is included

This project configures the AWS provider and the RHCS provider.It can create the networking layer, then provision the ROSA HCP cluster, account roles, operator roles, and OIDC resources.

Repository layout

main.tf contains the providers, locals, and ROSA module configuration.

variables.tf contains the input variables and defaults.

vpc.tf contains the optional VPC configuration.

Prerequisites

You need Terraform version 1.2.0 or later.

You need an AWS account with permissions to create IAM, networking, and ROSA resources.

You need a valid Red Hat OpenShift Cluster Manager token exported as an environment variable named RHCS_TOKEN.

Example command :

export RHCS_TOKEN = "your-rhcs-token"

Quick start

1.Initialize Terraform.

terraform init

2.Create a terraform.tfvars file.

Example values when creating a new VPC :

create_vpc      = true
private_cluster = false
aws_region      = "us-east-2"
cluster_name    = "rosa-hcp-demo"
multi_az        = true
default_aws_tags = {
  Environment = "dev"
  Owner       = "sunil"
}

Example values when using existing subnets :

create_vpc      = false
private_cluster = false
aws_region      = "us-east-2"
cluster_name    = "rosa-hcp-demo"
aws_subnet_ids  = ["subnet-abc123", "subnet-def456", "subnet-ghi789"]
default_aws_tags = {
  Environment = "dev"
  Owner       = "sunil"
}

3.Validate and review the plan.

terraform validate
terraform plan

4.Apply the configuration.

terraform apply

Notes

The current ROSA module version in this repository is 1.6.3.

This module uses Terraform lifecycle preconditions, so older Terraform versions will fail during initialization or validation.

Downloaded modules and local Terraform state should not be committed.

Recommended files to commit

Commit main.tf, variables.tf, vpc.tf, README.md,.gitignore, and.terraform.lock.hcl.

Files to exclude from Git

Do not commit.terraform, terraform.tfstate, terraform.tfstate backup files, crash logs, or local override files.

License

The Terraform files in this repository retain the upstream license headers included in the source.