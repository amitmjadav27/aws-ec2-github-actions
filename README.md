# aws-ec2-github-actions
To deploy EC2 Intance on AWS using github actions
# AWS EC2 Deployment using Terraform and GitHub Actions

## Overview

This project demonstrates Infrastructure as Code (IaC) using Terraform and GitHub Actions to provision AWS resources automatically.

The GitHub Actions workflow authenticates with AWS, initializes Terraform, validates the configuration, generates a Terraform plan, and deploys infrastructure to AWS.

## Architecture

```text
GitHub Repository
       |
       v
GitHub Actions Workflow
       |
       v
Terraform
       |
       v
AWS Infrastructure
       |
       +-- EC2 Instance
       +-- Security Group
```

## Features

* Automated infrastructure deployment using GitHub Actions
* Infrastructure provisioning using Terraform
* AWS EC2 instance creation
* Security Group creation and management
* GitHub Secrets integration for AWS credentials
* Manual infrastructure destruction workflow
* Support for Terraform remote backend (S3 + DynamoDB)

## Repository Structure

```text
.
├── .github
│   └── workflows
│       ├── deploy.yml
│       └── destroy.yml
│
├── main.tf
├── provider.tf
├── variables.tf
├── outputs.tf
├── backend.tf
└── README.md
```

## Prerequisites

Before using this project, ensure you have:

* AWS Account
* GitHub Repository
* IAM User with appropriate permissions
* Terraform knowledge (basic)
* Existing AWS Key Pair (optional for SSH access)

## GitHub Secrets

Configure the following secrets in your GitHub repository:

Settings → Secrets and Variables → Actions

| Secret Name           | Description           |
| --------------------- | --------------------- |
| AWS_ACCESS_KEY_ID     | AWS Access Key        |
| AWS_SECRET_ACCESS_KEY | AWS Secret Access Key |

## Deployment Workflow

The deployment workflow is triggered automatically when code is pushed to the main branch.

### Workflow Steps

1. Checkout repository
2. Configure AWS credentials
3. Install Terraform
4. Run Terraform Init
5. Run Terraform Validate
6. Run Terraform Plan
7. Run Terraform Apply

### Deploy Infrastructure

```bash
git add .
git commit -m "Deploy infrastructure"
git push origin main
```

## Destroy Infrastructure

The destroy workflow can be triggered manually from GitHub Actions.

### Steps

1. Navigate to Actions tab
2. Select "Destroy Terraform Infrastructure"
3. Click "Run Workflow"
4. Confirm execution

Terraform will remove all resources managed by the state file.

## Terraform Remote State (Recommended)

Store Terraform state remotely using S3.

### S3 Backend

```hcl
terraform {
  backend "s3" {
    bucket         = "your-terraform-state-bucket"
    key            = "ec2/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-lock"
  }
}
```

### Benefits

* Persistent state storage
* Team collaboration
* State locking
* Disaster recovery

## SSH Access

If a key pair is attached to the EC2 instance:

Amazon Linux:

```bash
ssh -i my-key.pem ec2-user@<public-ip>
```

Ubuntu:

```bash
ssh -i my-key.pem ubuntu@<public-ip>
```

## Common Troubleshooting

### No Configuration Files

Cause:

```text
Terraform cannot locate .tf files.
```

Solution:

* Verify Terraform files exist
* Ensure actions/checkout step is present
* Verify working directory configuration

### No Resources Destroyed

Cause:

```text
Terraform state file not available.
```

Solution:

* Configure S3 backend
* Verify state file exists
* Run terraform init

### Access Denied

Cause:

```text
AWS credentials or IAM permissions issue.
```

Solution:

* Verify GitHub Secrets
* Check IAM policies

## Future Enhancements

* AWS OIDC Authentication
* Terraform Modules
* Multi-Environment Deployment
* Auto Scaling Group
* Application Load Balancer
* Packer AMI Creation
* Ansible Configuration Management
* ECS/EKS Deployment

## Author

Amit Jadav

DevOps | AWS Cloud | Terraform | GitHub Actions | CI/CD
