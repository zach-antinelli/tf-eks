# Terraform EKS Cluster

Terraform AWS EKS cluster deployment with secure and cost effective configuration. With defaults, the resources provisioned are a minimal deployment with a low node count and volume size to cut down on costs for a dev environment.

## Prerequisites

- [Terraform](https://www.terraform.io/downloads.html)
  - `brew install terraform`
- [AWS CLI](https://aws.amazon.com/cli)
  - `brew install awscli`
  - With appropriate permissions:
    - EKS cluster creation
    - IAM role and policy management
    - VPC and networking resources
- [kubectl](https://kubernetes.io/docs/reference/kubectl)
  - `brew install kubectl`

## Features

This module includes the following configuration:

1. Addons
   1. Core DNS
   2. EBS CSI Driver
   3. EKS Pod Identity Agent
   4. IAM Roles for Service Accounts (IRSA)
   5. Kube Proxy
   6. VPC CNI
2. KMS encryption for EKS secrets
    - 7 day key rotation
3. IMDSv2 for EC2 metadata access
4. Mix of restricted public API access and private access
   - Public access restricted to your public IP cidr
   - Private access within VPC
5. S3 and ECR VPC endpoints
6. Multi AZ deployment for high availability
7. Cloudwatch logging with 7 day retention
8. Cost savings
    - Graviton (ARM) EC2 workers
    - Using spot instances by default
    - Single NAT Gateway

## Deployment

1. Clone this repository
2. Modify [terraform.tfvars](terraform.tfvars) as needed and deploy the module.
3. Deploy the module

```bash
terraform init
terraform plan
terraform apply
```

## Notes

- The public IP CIDR assigned to the cluster API public access is automatically determined by querying `https://checkip.amazonaws.com` in [locals.tf](/locals.tf)
- The IAM user running the terraform module will be added as an admin to the EKS cluster.
- Due to the addons, the module may take some time to fully deploy (around 15 minutes).
