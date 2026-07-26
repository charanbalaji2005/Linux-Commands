# ☁️ The Complete AWS CLI & Cloud Handbook

[![AWS](https://img.shields.io/badge/AWS-Cloud-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![CLI](https://img.shields.io/badge/AWS-CLI%20v2-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)](https://docs.aws.amazon.com/cli/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

---

## 📌 Table of Contents
* [1. AWS CLI Setup & Configuration](#1-aws-cli-setup--configuration)
* [2. IAM (Identity & Access Management)](#2-iam-identity--access-management)
* [3. EC2 (Elastic Compute Cloud)](#3-ec2-elastic-compute-cloud)
* [4. S3 (Simple Storage Service)](#4-s3-simple-storage-service)
* [5. VPC & Networking](#5-vpc--networking)
* [6. RDS (Relational Database Service)](#6-rds-relational-database-service)
* [7. Lambda & Serverless](#7-lambda--serverless)
* [8. ECS, ECR & EKS (Containers)](#8-ecs-ecr--eks-containers)
* [9. CloudWatch (Monitoring & Logs)](#9-cloudwatch-monitoring--logs)
* [10. CloudFormation & Infrastructure as Code](#10-cloudformation--infrastructure-as-code)
* [11. Route 53, ACM & CDN](#11-route-53-acm--cdn)
* [12. Cost & Billing](#12-cost--billing)

---

## 1. AWS CLI Setup & Configuration

```bash
# Install AWS CLI v2 (Linux)
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Verify installation
aws --version

# Configure credentials (Access Key, Secret Key, Region, Output format)
aws configure

# Configure a named profile
aws configure --profile devuser

# Use a specific profile for a command
aws s3 ls --profile devuser

# List all configured profiles
aws configure list-profiles

# View current identity (account, user, ARN)
aws sts get-caller-identity
```

---

## 2. IAM (Identity & Access Management)

```bash
# List all IAM users
aws iam list-users

# Create a new IAM user
aws iam create-user --user-name devuser

# Create an access key for a user
aws iam create-access-key --user-name devuser

# Attach a managed policy to a user
aws iam attach-user-policy --user-name devuser \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

# Create an IAM role (with trust policy JSON)
aws iam create-role --role-name ec2-app-role \
  --assume-role-policy-document file://trust-policy.json

# List all roles
aws iam list-roles

# Create and attach an inline policy
aws iam put-user-policy --user-name devuser \
  --policy-name S3ReadOnly --policy-document file://policy.json

# Delete a user (must remove keys/policies first)
aws iam delete-user --user-name devuser
```

---

## 3. EC2 (Elastic Compute Cloud)

```bash
# Launch a new EC2 instance
aws ec2 run-instances \
  --image-id ami-0abcdef1234567890 \
  --instance-type t3.micro \
  --key-name my-key \
  --security-group-ids sg-0123456789 \
  --subnet-id subnet-0123456789 \
  --count 1

# List all instances (with status)
aws ec2 describe-instances \
  --query "Reservations[].Instances[].{ID:InstanceId,State:State.Name,IP:PublicIpAddress}" \
  --output table

# Start / Stop / Reboot / Terminate an instance
aws ec2 start-instances --instance-ids i-0123456789abcdef0
aws ec2 stop-instances --instance-ids i-0123456789abcdef0
aws ec2 reboot-instances --instance-ids i-0123456789abcdef0
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0

# Create a key pair for SSH access
aws ec2 create-key-pair --key-name my-key \
  --query 'KeyMaterial' --output text > my-key.pem
chmod 400 my-key.pem

# Create a security group and open a port
aws ec2 create-security-group --group-name web-sg --description "Web access"
aws ec2 authorize-security-group-ingress \
  --group-name web-sg --protocol tcp --port 22 --cidr 0.0.0.0/0

# List available AMIs owned by you
aws ec2 describe-images --owners self

# Create an AMI snapshot of a running instance
aws ec2 create-image --instance-id i-0123456789abcdef0 \
  --name "my-app-backup" --no-reboot
```

---

## 4. S3 (Simple Storage Service)

```bash
# Create a new bucket
aws s3 mb s3://my-app-bucket-2026

# List all buckets
aws s3 ls

# List objects inside a bucket
aws s3 ls s3://my-app-bucket-2026 --recursive

# Upload a file
aws s3 cp ./build/index.html s3://my-app-bucket-2026/

# Upload an entire folder (sync, only changed files)
aws s3 sync ./dist s3://my-app-bucket-2026 --delete

# Download a file
aws s3 cp s3://my-app-bucket-2026/report.pdf ./report.pdf

# Make an object public
aws s3api put-object-acl --bucket my-app-bucket-2026 \
  --key index.html --acl public-read

# Enable static website hosting
aws s3 website s3://my-app-bucket-2026/ \
  --index-document index.html --error-document error.html

# Delete a bucket (must be empty first)
aws s3 rm s3://my-app-bucket-2026 --recursive
aws s3 rb s3://my-app-bucket-2026

# Enable versioning on a bucket
aws s3api put-bucket-versioning --bucket my-app-bucket-2026 \
  --versioning-configuration Status=Enabled
```

---

## 5. VPC & Networking

```bash
# Create a new VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Create a subnet inside a VPC
aws ec2 create-subnet --vpc-id vpc-0123456789 \
  --cidr-block 10.0.1.0/24 --availability-zone ap-south-1a

# Create and attach an internet gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --vpc-id vpc-0123456789 \
  --internet-gateway-id igw-0123456789

# Create a route table and add a public route
aws ec2 create-route-table --vpc-id vpc-0123456789
aws ec2 create-route --route-table-id rtb-0123456789 \
  --destination-cidr-block 0.0.0.0/0 --gateway-id igw-0123456789

# List all VPCs
aws ec2 describe-vpcs

# List all subnets in a VPC
aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-0123456789"

# Allocate and associate an Elastic IP
aws ec2 allocate-address
aws ec2 associate-address --instance-id i-0123456789abcdef0 \
  --allocation-id eipalloc-0123456789
```

---

## 6. RDS (Relational Database Service)

```bash
# Create a new RDS instance (PostgreSQL example)
aws rds create-db-instance \
  --db-instance-identifier my-app-db \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username admin \
  --master-user-password 'ChangeMe123!' \
  --allocated-storage 20

# List all DB instances
aws rds describe-db-instances \
  --query "DBInstances[].{ID:DBInstanceIdentifier,Status:DBInstanceStatus,Endpoint:Endpoint.Address}" \
  --output table

# Create a manual snapshot
aws rds create-db-snapshot \
  --db-instance-identifier my-app-db \
  --db-snapshot-identifier my-app-db-snapshot-2026

# Restore from a snapshot
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier my-app-db-restored \
  --db-snapshot-identifier my-app-db-snapshot-2026

# Delete a DB instance
aws rds delete-db-instance \
  --db-instance-identifier my-app-db \
  --skip-final-snapshot
```

---

## 7. Lambda & Serverless

```bash
# Create a new Lambda function from a zipped package
aws lambda create-function \
  --function-name myAppFunction \
  --runtime nodejs20.x \
  --role arn:aws:iam::123456789012:role/lambda-exec-role \
  --handler index.handler \
  --zip-file fileb://function.zip

# Invoke a function and view the response
aws lambda invoke --function-name myAppFunction output.json
cat output.json

# Update function code
aws lambda update-function-code \
  --function-name myAppFunction --zip-file fileb://function.zip

# List all functions
aws lambda list-functions --query "Functions[].FunctionName"

# Add an environment variable
aws lambda update-function-configuration \
  --function-name myAppFunction \
  --environment "Variables={STAGE=production}"

# Delete a function
aws lambda delete-function --function-name myAppFunction
```

---

## 8. ECS, ECR & EKS (Containers)

```bash
# --- ECR (Elastic Container Registry) ---
aws ecr create-repository --repository-name my-app
aws ecr get-login-password --region ap-south-1 | \
  docker login --username AWS --password-stdin 123456789012.dkr.ecr.ap-south-1.amazonaws.com
docker tag my-app:latest 123456789012.dkr.ecr.ap-south-1.amazonaws.com/my-app:latest
docker push 123456789012.dkr.ecr.ap-south-1.amazonaws.com/my-app:latest

# --- ECS (Elastic Container Service) ---
aws ecs create-cluster --cluster-name my-cluster
aws ecs register-task-definition --cli-input-json file://task-def.json
aws ecs run-task --cluster my-cluster --task-definition my-task
aws ecs list-tasks --cluster my-cluster
aws ecs update-service --cluster my-cluster --service my-service --desired-count 3

# --- EKS (Elastic Kubernetes Service) ---
aws eks create-cluster --name my-eks-cluster \
  --role-arn arn:aws:iam::123456789012:role/eks-cluster-role \
  --resources-vpc-config subnetIds=subnet-0123,subnet-0456
aws eks update-kubeconfig --name my-eks-cluster --region ap-south-1
aws eks list-clusters
kubectl get nodes
```

---

## 9. CloudWatch (Monitoring & Logs)

```bash
# List all log groups
aws logs describe-log-groups

# Tail a log group's recent events
aws logs tail /aws/lambda/myAppFunction --follow

# Get CPU utilization metric for an EC2 instance
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-0123456789abcdef0 \
  --start-time 2026-07-26T00:00:00Z --end-time 2026-07-27T00:00:00Z \
  --period 3600 --statistics Average

# Create an alarm (e.g., high CPU)
aws cloudwatch put-metric-alarm \
  --alarm-name high-cpu-alert \
  --metric-name CPUUtilization --namespace AWS/EC2 \
  --statistic Average --period 300 --threshold 80 \
  --comparison-operator GreaterThanThreshold --evaluation-periods 2 \
  --dimensions Name=InstanceId,Value=i-0123456789abcdef0

# List all active alarms
aws cloudwatch describe-alarms
```

---

## 10. CloudFormation & Infrastructure as Code

```bash
# Validate a template before deploying
aws cloudformation validate-template --template-body file://stack.yaml

# Create a new stack
aws cloudformation create-stack \
  --stack-name my-app-stack \
  --template-body file://stack.yaml \
  --capabilities CAPABILITY_IAM

# Update an existing stack
aws cloudformation update-stack \
  --stack-name my-app-stack --template-body file://stack.yaml

# Check stack status
aws cloudformation describe-stacks --stack-name my-app-stack \
  --query "Stacks[0].StackStatus"

# Delete a stack (removes all its resources)
aws cloudformation delete-stack --stack-name my-app-stack

# Deploy using Terraform (alternative IaC tool)
terraform init
terraform plan
terraform apply
terraform destroy
```

---

## 11. Route 53, ACM & CDN

```bash
# List hosted zones (DNS)
aws route53 list-hosted-zones

# Create a DNS record (A record)
aws route53 change-resource-record-sets \
  --hosted-zone-id Z1234567890 \
  --change-batch file://record-change.json

# Request an SSL/TLS certificate (ACM)
aws acm request-certificate \
  --domain-name example.com \
  --validation-method DNS

# List certificates
aws acm list-certificates

# Create a CloudFront distribution (CDN)
aws cloudfront create-distribution \
  --origin-domain-name my-app-bucket-2026.s3.amazonaws.com

# Invalidate CloudFront cache after a deploy
aws cloudfront create-invalidation \
  --distribution-id E1234567890ABC --paths "/*"
```

---

## 12. Cost & Billing

```bash
# Get total cost for the current month
aws ce get-cost-and-usage \
  --time-period Start=2026-07-01,End=2026-07-27 \
  --granularity MONTHLY --metrics "UnblendedCost"

# List cost by service
aws ce get-cost-and-usage \
  --time-period Start=2026-07-01,End=2026-07-27 \
  --granularity MONTHLY --metrics "UnblendedCost" \
  --group-by Type=DIMENSION,Key=SERVICE

# Get current account budgets
aws budgets describe-budgets --account-id 123456789012

# Check free tier usage alerts
aws ce get-cost-forecast \
  --time-period Start=2026-07-27,End=2026-08-27 \
  --metric UNBLENDED_COST --granularity MONTHLY
```

---

## 📄 License
This handbook is released under the [MIT License](https://opensource.org/licenses/MIT) — free to use, copy, and share.

