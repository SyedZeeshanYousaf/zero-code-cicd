# Zero-Code AWS CI/CD Pipeline (GitHub Actions + Amazon ECR + EC2)

Automated CI/CD deployment pipeline deploying a lightweight `nginx:alpine` container to an Ubuntu AWS EC2 instance using secretless AWS IAM OIDC authentication and Amazon ECR.

## Architecture
`Local Push (main)` -> `GitHub Actions (OIDC Auth)` -> `Amazon ECR (Private Image)` -> `AWS EC2 Ubuntu (Docker Compose)`

## Required GitHub Repository Secrets
Add these under **Settings > Secrets and variables > Actions**:
- `AWS_ROLE_ARN`: ARN of your IAM Role configured with GitHub OIDC (`arn:aws:iam::<ACCOUNT_ID>:role/GitHubActions-ECR-Role`)
- `EC2_HOST`: Public IPv4 address of your Ubuntu EC2 instance
- `EC2_USER`: `ubuntu`
- `EC2_SSH_KEY`: Full `.pem` private key contents

## AWS Setup Checklist
1. Create a private Amazon ECR repository named `zero-code-cicd` in `us-east-1` (or update `AWS_REGION` in `.github/workflows/deploy.yml` to match your repository's region).
2. Attach an IAM role with `AmazonEC2ContainerRegistryReadOnly` to your Ubuntu EC2 instance.
3. Ensure Docker, Docker Compose v2, and AWS CLI v2 are installed on your EC2 instance, and port `80` (HTTP) is open in its Security Group.
