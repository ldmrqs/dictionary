# AWS CLI Dictionary

A complete reference of AWS CLI commands in a direct and straightforward way.

## Table of Contents

- [Setup and Configuration](#setup-and-configuration)
- [EC2 (Virtual Machines)](#ec2-virtual-machines)
- [S3 (Storage)](#s3-storage)
- [IAM (Identity and Access)](#iam-identity-and-access)
- [Lambda (Serverless Functions)](#lambda-serverless-functions)
- [RDS (Databases)](#rds-databases)
- [VPC (Networking)](#vpc-networking)
- [ECS (Containers)](#ecs-containers)
- [CloudFormation (Infrastructure)](#cloudformation-infrastructure)
- [CloudWatch (Monitoring)](#cloudwatch-monitoring)
- [Route53 (DNS)](#route53-dns)
- [ELB (Load Balancers)](#elb-load-balancers)
- [Auto Scaling](#auto-scaling)
- [SQS (Queues)](#sqs-queues)
- [SNS (Notifications)](#sns-notifications)
- [Common Patterns](#common-patterns)

---

## Setup and Configuration

Getting AWS CLI ready to use:

| Command | Meaning |
|---------|---------|
| `aws configure` | set up credentials and region |
| `aws configure list` | show current configuration |
| `aws configure --profile <name>` | configure named profile |
| `aws sts get-caller-identity` | show current user/role |
| `aws sts assume-role` | assume IAM role |
| `aws configure set <key> <value>` | set specific config value |
| `aws configure get <key>` | get specific config value |

**Setup examples:**
```bash
# Initial setup
aws configure                    # enter access key, secret, region, output format

# Multiple profiles
aws configure --profile dev      # create dev profile
aws configure --profile prod     # create prod profile

# Use specific profile
aws s3 ls --profile dev          # use dev profile for command
export AWS_PROFILE=dev           # set default profile for session

# Check current identity
aws sts get-caller-identity      # see who you are
aws sts get-caller-identity --profile prod

# Set region for current session
export AWS_DEFAULT_REGION=us-west-2

# Configure specific values
aws configure set region us-east-1
aws configure set output json
aws configure set cli_pager ""   # disable pager
```

---

## EC2 (Virtual Machines)

Managing EC2 instances:

| Command | Meaning |
|---------|---------|
| `aws ec2 describe-instances` | list all instances |
| `aws ec2 run-instances` | launch new instance |
| `aws ec2 start-instances` | start stopped instances |
| `aws ec2 stop-instances` | stop running instances |
| `aws ec2 terminate-instances` | permanently delete instances |
| `aws ec2 describe-images` | list available AMIs |
| `aws ec2 create-key-pair` | create SSH key pair |
| `aws ec2 describe-security-groups` | list security groups |

**EC2 examples:**
```bash
# List instances
aws ec2 describe-instances                                    # all instances
aws ec2 describe-instances --instance-ids i-1234567890abcdef0 # specific instance
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"

# Launch instance
aws ec2 run-instances \
  --image-id ami-12345678 \
  --count 1 \
  --instance-type t2.micro \
  --key-name my-key \
  --security-group-ids sg-12345678 \
  --subnet-id subnet-12345678

# Start/stop instances
aws ec2 start-instances --instance-ids i-1234567890abcdef0
aws ec2 stop-instances --instance-ids i-1234567890abcdef0
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0

# Get instance info in table format
aws ec2 describe-instances \
  --query 'Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType,PublicIpAddress]' \
  --output table

# Find latest Amazon Linux AMI
aws ec2 describe-images \
  --owners amazon \
  --filters "Name=name,Values=amzn2-ami-hvm-*" \
  --query 'Images | sort_by(@, &CreationDate) | [-1].ImageId' \
  --output text

# Create key pair
aws ec2 create-key-pair --key-name my-key --output text > my-key.pem
chmod 400 my-key.pem

# Security groups
aws ec2 describe-security-groups
aws ec2 create-security-group --group-name web-sg --description "Web servers"
aws ec2 authorize-security-group-ingress \
  --group-name web-sg \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0
```

---

## S3 (Storage)

Managing S3 buckets and objects:

| Command | Meaning |
|---------|---------|
| `aws s3 ls` | list buckets and objects |
| `aws s3 mb s3://bucket` | create bucket |
| `aws s3 rb s3://bucket` | delete bucket |
| `aws s3 cp <source> <dest>` | copy files |
| `aws s3 sync <source> <dest>` | sync directories |
| `aws s3 rm s3://bucket/key` | delete object |
| `aws s3api` | advanced S3 operations |

**S3 examples:**
```bash
# List buckets and contents
aws s3 ls                        # list all buckets
aws s3 ls s3://my-bucket         # list objects in bucket
aws s3 ls s3://my-bucket/folder/ --recursive

# Create and delete buckets
aws s3 mb s3://my-unique-bucket-name
aws s3 rb s3://my-bucket         # empty bucket
aws s3 rb s3://my-bucket --force # delete bucket and all contents

# Upload/download files
aws s3 cp file.txt s3://my-bucket/
aws s3 cp s3://my-bucket/file.txt ./downloaded-file.txt
aws s3 cp folder/ s3://my-bucket/folder/ --recursive

# Sync directories
aws s3 sync ./local-folder s3://my-bucket/remote-folder/
aws s3 sync s3://my-bucket/remote-folder/ ./local-folder
aws s3 sync ./website s3://my-bucket --delete  # delete files not in source

# Delete objects
aws s3 rm s3://my-bucket/file.txt
aws s3 rm s3://my-bucket/folder/ --recursive

# Advanced operations with s3api
aws s3api get-object --bucket my-bucket --key file.txt downloaded-file.txt
aws s3api put-object --bucket my-bucket --key file.txt --body file.txt
aws s3api list-object-versions --bucket my-bucket

# Set bucket policy
aws s3api put-bucket-policy --bucket my-bucket --policy file://policy.json

# Enable versioning
aws s3api put-bucket-versioning --bucket my-bucket --versioning-configuration Status=Enabled

# Website hosting
aws s3 website s3://my-bucket --index-document index.html --error-document error.html
```

---

## IAM (Identity and Access)

Managing users, roles, and permissions:

| Command | Meaning |
|---------|---------|
| `aws iam list-users` | list all users |
| `aws iam create-user` | create new user |
| `aws iam list-roles` | list all roles |
| `aws iam create-role` | create new role |
| `aws iam list-policies` | list policies |
| `aws iam attach-user-policy` | attach policy to user |
| `aws iam create-access-key` | create access keys |

**IAM examples:**
```bash
# Users
aws iam list-users
aws iam create-user --user-name john
aws iam delete-user --user-name john

# Get user details
aws iam get-user --user-name john
aws iam list-attached-user-policies --user-name john

# Access keys
aws iam create-access-key --user-name john
aws iam list-access-keys --user-name john
aws iam delete-access-key --user-name john --access-key-id AKIAIOSFODNN7EXAMPLE

# Roles
aws iam list-roles
aws iam get-role --role-name MyRole
aws iam create-role --role-name MyRole --assume-role-policy-document file://trust-policy.json

# Policies
aws iam list-policies --scope Local        # custom policies
aws iam list-policies --scope AWS          # AWS managed policies
aws iam get-policy --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess

# Attach policies
aws iam attach-user-policy --user-name john --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
aws iam attach-role-policy --role-name MyRole --policy-arn arn:aws:iam::aws:policy/S3ReadOnlyAccess

# Groups
aws iam create-group --group-name developers
aws iam add-user-to-group --user-name john --group-name developers
aws iam attach-group-policy --group-name developers --policy-arn arn:aws:iam::aws:policy/PowerUserAccess

# Password policy
aws iam get-account-password-policy
aws iam update-account-password-policy --minimum-password-length 12 --require-symbols
```

---

## Lambda (Serverless Functions)

Managing Lambda functions:

| Command | Meaning |
|---------|---------|
| `aws lambda list-functions` | list all functions |
| `aws lambda create-function` | create new function |
| `aws lambda invoke` | execute function |
| `aws lambda update-function-code` | update function code |
| `aws lambda get-function` | get function details |
| `aws lambda delete-function` | delete function |

**Lambda examples:**
```bash
# List functions
aws lambda list-functions
aws lambda list-functions --query 'Functions[*].[FunctionName,Runtime,LastModified]' --output table

# Create function
aws lambda create-function \
  --function-name my-function \
  --runtime python3.9 \
  --role arn:aws:iam::123456789012:role/lambda-role \
  --handler index.handler \
  --zip-file fileb://function.zip

# Invoke function
aws lambda invoke --function-name my-function response.json
aws lambda invoke --function-name my-function --payload '{"key":"value"}' response.json

# Update function
aws lambda update-function-code --function-name my-function --zip-file fileb://new-function.zip
aws lambda update-function-configuration --function-name my-function --timeout 30

# Get function info
aws lambda get-function --function-name my-function
aws lambda get-function-configuration --function-name my-function

# Environment variables
aws lambda update-function-configuration \
  --function-name my-function \
  --environment Variables='{ENV=prod,DEBUG=false}'

# Create deployment package
zip -r function.zip index.py
aws lambda update-function-code --function-name my-function --zip-file fileb://function.zip

# Delete function
aws lambda delete-function --function-name my-function
```

---

## RDS (Databases)

Managing RDS database instances:

| Command | Meaning |
|---------|---------|
| `aws rds describe-db-instances` | list database instances |
| `aws rds create-db-instance` | create database |
| `aws rds delete-db-instance` | delete database |
| `aws rds start-db-instance` | start stopped database |
| `aws rds stop-db-instance` | stop running database |
| `aws rds create-db-snapshot` | create database snapshot |

**RDS examples:**
```bash
# List databases
aws rds describe-db-instances
aws rds describe-db-instances --query 'DBInstances[*].[DBInstanceIdentifier,DBInstanceStatus,Engine]' --output table

# Create database
aws rds create-db-instance \
  --db-instance-identifier mydb \
  --db-instance-class db.t3.micro \
  --engine mysql \
  --master-username admin \
  --master-user-password mypassword \
  --allocated-storage 20

# Database operations
aws rds start-db-instance --db-instance-identifier mydb
aws rds stop-db-instance --db-instance-identifier mydb
aws rds reboot-db-instance --db-instance-identifier mydb

# Snapshots
aws rds create-db-snapshot --db-instance-identifier mydb --db-snapshot-identifier mydb-snapshot
aws rds describe-db-snapshots --db-instance-identifier mydb
aws rds delete-db-snapshot --db-snapshot-identifier mydb-snapshot

# Restore from snapshot
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier mydb-restored \
  --db-snapshot-identifier mydb-snapshot

# Get connection endpoint
aws rds describe-db-instances \
  --db-instance-identifier mydb \
  --query 'DBInstances[0].Endpoint.Address' \
  --output text

# Delete database
aws rds delete-db-instance \
  --db-instance-identifier mydb \
  --skip-final-snapshot
```

---

## VPC (Networking)

Managing Virtual Private Clouds:

| Command | Meaning |
|---------|---------|
| `aws ec2 describe-vpcs` | list VPCs |
| `aws ec2 create-vpc` | create VPC |
| `aws ec2 describe-subnets` | list subnets |
| `aws ec2 create-subnet` | create subnet |
| `aws ec2 describe-route-tables` | list route tables |
| `aws ec2 create-internet-gateway` | create internet gateway |

**VPC examples:**
```bash
# VPCs
aws ec2 describe-vpcs
aws ec2 create-vpc --cidr-block 10.0.0.0/16
aws ec2 delete-vpc --vpc-id vpc-12345678

# Subnets
aws ec2 describe-subnets
aws ec2 create-subnet --vpc-id vpc-12345678 --cidr-block 10.0.1.0/24 --availability-zone us-west-2a
aws ec2 delete-subnet --subnet-id subnet-12345678

# Internet Gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --internet-gateway-id igw-12345678 --vpc-id vpc-12345678
aws ec2 detach-internet-gateway --internet-gateway-id igw-12345678 --vpc-id vpc-12345678

# Route Tables
aws ec2 describe-route-tables
aws ec2 create-route-table --vpc-id vpc-12345678
aws ec2 create-route --route-table-id rtb-12345678 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-12345678
aws ec2 associate-route-table --route-table-id rtb-12345678 --subnet-id subnet-12345678

# Security Groups
aws ec2 create-security-group --group-name web-sg --description "Web Security Group" --vpc-id vpc-12345678
aws ec2 authorize-security-group-ingress \
  --group-id sg-12345678 \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0

# NAT Gateway
aws ec2 create-nat-gateway --subnet-id subnet-12345678 --allocation-id eipalloc-12345678
aws ec2 describe-nat-gateways

# VPC Endpoints
aws ec2 create-vpc-endpoint \
  --vpc-id vpc-12345678 \
  --service-name com.amazonaws.us-west-2.s3 \
  --route-table-ids rtb-12345678
```

---

## ECS (Containers)

Managing container services:

| Command | Meaning |
|---------|---------|
| `aws ecs list-clusters` | list ECS clusters |
| `aws ecs create-cluster` | create cluster |
| `aws ecs list-services` | list services in cluster |
| `aws ecs create-service` | create service |
| `aws ecs list-tasks` | list running tasks |
| `aws ecs run-task` | run one-time task |

**ECS examples:**
```bash
# Clusters
aws ecs list-clusters
aws ecs create-cluster --cluster-name my-cluster
aws ecs delete-cluster --cluster my-cluster

# Task Definitions
aws ecs list-task-definitions
aws ecs register-task-definition --cli-input-json file://task-definition.json
aws ecs describe-task-definition --task-definition my-app:1

# Services
aws ecs list-services --cluster my-cluster
aws ecs create-service \
  --cluster my-cluster \
  --service-name my-service \
  --task-definition my-app:1 \
  --desired-count 2

aws ecs update-service --cluster my-cluster --service my-service --desired-count 3
aws ecs delete-service --cluster my-cluster --service my-service --force

# Tasks
aws ecs list-tasks --cluster my-cluster
aws ecs run-task --cluster my-cluster --task-definition my-app:1 --count 1
aws ecs stop-task --cluster my-cluster --task arn:aws:ecs:...

# Container Instances
aws ecs list-container-instances --cluster my-cluster
aws ecs describe-container-instances --cluster my-cluster --container-instances arn:aws:ecs:...
```

---

## CloudFormation (Infrastructure)

Managing infrastructure as code:

| Command | Meaning |
|---------|---------|
| `aws cloudformation list-stacks` | list all stacks |
| `aws cloudformation create-stack` | create new stack |
| `aws cloudformation update-stack` | update existing stack |
| `aws cloudformation delete-stack` | delete stack |
| `aws cloudformation describe-stacks` | get stack details |
| `aws cloudformation validate-template` | validate template |

**CloudFormation examples:**
```bash
# List stacks
aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE
aws cloudformation describe-stacks

# Create stack
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --parameters ParameterKey=InstanceType,ParameterValue=t2.micro \
  --capabilities CAPABILITY_IAM

# Update stack
aws cloudformation update-stack \
  --stack-name my-stack \
  --template-body file://updated-template.yaml \
  --parameters ParameterKey=InstanceType,ParameterValue=t2.small

# Delete stack
aws cloudformation delete-stack --stack-name my-stack

# Get stack outputs
aws cloudformation describe-stacks \
  --stack-name my-stack \
  --query 'Stacks[0].Outputs'

# Validate template
aws cloudformation validate-template --template-body file://template.yaml

# Stack events
aws cloudformation describe-stack-events --stack-name my-stack

# Wait for completion
aws cloudformation wait stack-create-complete --stack-name my-stack
aws cloudformation wait stack-update-complete --stack-name my-stack

# Drift detection
aws cloudformation detect-stack-drift --stack-name my-stack
aws cloudformation describe-stack-drift-detection-status --stack-drift-detection-id detection-id
```

---

## CloudWatch (Monitoring)

Managing logs and metrics:

| Command | Meaning |
|---------|---------|
| `aws logs describe-log-groups` | list log groups |
| `aws logs create-log-group` | create log group |
| `aws logs describe-log-streams` | list log streams |
| `aws logs get-log-events` | get log events |
| `aws cloudwatch list-metrics` | list available metrics |
| `aws cloudwatch put-metric-data` | send custom metrics |

**CloudWatch examples:**
```bash
# Log Groups
aws logs describe-log-groups
aws logs create-log-group --log-group-name /aws/lambda/my-function
aws logs delete-log-group --log-group-name /aws/lambda/my-function

# Log Streams
aws logs describe-log-streams --log-group-name /aws/lambda/my-function
aws logs get-log-events \
  --log-group-name /aws/lambda/my-function \
  --log-stream-name '2023/07/13/[$LATEST]...'

# Filter logs
aws logs filter-log-events \
  --log-group-name /aws/lambda/my-function \
  --filter-pattern "ERROR" \
  --start-time 1689206400000

# Metrics
aws cloudwatch list-metrics --namespace AWS/EC2
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
  --start-time 2023-07-13T00:00:00Z \
  --end-time 2023-07-13T23:59:59Z \
  --period 3600 \
  --statistics Average

# Custom metrics
aws cloudwatch put-metric-data \
  --namespace MyApp \
  --metric-data MetricName=PageViews,Value=100,Unit=Count

# Alarms
aws cloudwatch describe-alarms
aws cloudwatch put-metric-alarm \
  --alarm-name high-cpu \
  --alarm-description "High CPU usage" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold
```

---

## Route53 (DNS)

Managing DNS and domain names:

| Command | Meaning |
|---------|---------|
| `aws route53 list-hosted-zones` | list hosted zones |
| `aws route53 create-hosted-zone` | create hosted zone |
| `aws route53 list-resource-record-sets` | list DNS records |
| `aws route53 change-resource-record-sets` | create/update DNS records |

**Route53 examples:**
```bash
# Hosted Zones
aws route53 list-hosted-zones
aws route53 create-hosted-zone --name example.com --caller-reference $(date +%s)

# DNS Records
aws route53 list-resource-record-sets --hosted-zone-id Z123456789
aws route53 change-resource-record-sets \
  --hosted-zone-id Z123456789 \
  --change-batch file://change-batch.json

# Example change-batch.json for A record:
{
  "Changes": [
    {
      "Action": "CREATE",
      "ResourceRecordSet": {
        "Name": "www.example.com",
        "Type": "A",
        "TTL": 300,
        "ResourceRecords": [
          {"Value": "192.0.2.44"}
        ]
      }
    }
  ]
}

# Health Checks
aws route53 list-health-checks
aws route53 create-health-check \
  --caller-reference $(date +%s) \
  --health-check-config Type=HTTP,ResourcePath=/health,FullyQualifiedDomainName=example.com
```

---

## ELB (Load Balancers)

Managing Elastic Load Balancers:

| Command | Meaning |
|---------|---------|
| `aws elbv2 describe-load-balancers` | list load balancers |
| `aws elbv2 create-load-balancer` | create load balancer |
| `aws elbv2 describe-target-groups` | list target groups |
| `aws elbv2 create-target-group` | create target group |
| `aws elbv2 register-targets` | add targets to group |

**ELB examples:**
```bash
# Load Balancers (ALB/NLB)
aws elbv2 describe-load-balancers
aws elbv2 create-load-balancer \
  --name my-load-balancer \
  --subnets subnet-12345678 subnet-87654321 \
  --security-groups sg-12345678

# Target Groups
aws elbv2 describe-target-groups
aws elbv2 create-target-group \
  --name my-targets \
  --protocol HTTP \
  --port 80 \
  --vpc-id vpc-12345678 \
  --health-check-path /health

# Register targets
aws elbv2 register-targets \
  --target-group-arn arn:aws:elasticloadbalancing:... \
  --targets Id=i-1234567890abcdef0,Port=80

# Listeners
aws elbv2 describe-listeners --load-balancer-arn arn:aws:elasticloadbalancing:...
aws elbv2 create-listener \
  --load-balancer-arn arn:aws:elasticloadbalancing:... \
  --protocol HTTP \
  --port 80 \
  --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:...

# Classic Load Balancer (older)
aws elb describe-load-balancers
aws elb create-load-balancer \
  --load-balancer-name my-lb \
  --listeners Protocol=HTTP,LoadBalancerPort=80,InstancePort=80 \
  --subnets subnet-12345678
```

---

## Auto Scaling

Managing Auto Scaling Groups:

| Command | Meaning |
|---------|---------|
| `aws autoscaling describe-auto-scaling-groups` | list auto scaling groups |
| `aws autoscaling create-auto-scaling-group` | create auto scaling group |
| `aws autoscaling describe-launch-configurations` | list launch configurations |
| `aws autoscaling create-launch-configuration` | create launch configuration |
| `aws autoscaling update-auto-scaling-group` | update group settings |

**Auto Scaling examples:**
```bash
# Launch Configurations
aws autoscaling describe-launch-configurations
aws autoscaling create-launch-configuration \
  --launch-configuration-name my-lc \
  --image-id ami-12345678 \
  --instance-type t2.micro \
  --security-groups sg-12345678 \
  --key-name my-key

# Auto Scaling Groups
aws autoscaling describe-auto-scaling-groups
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --launch-configuration-name my-lc \
  --min-size 1 \
  --max-size 3 \
  --desired-capacity 2 \
  --vpc-zone-identifier "subnet-12345678,subnet-87654321"

# Update capacity
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --desired-capacity 3

# Scaling Policies
aws autoscaling put-scaling-policy \
  --auto-scaling-group-name my-asg \
  --policy-name scale-up \
  --policy-type SimpleScaling \
  --adjustment-type ChangeInCapacity \
  --scaling-adjustment 1

# Attach to Load Balancer
aws autoscaling attach-load-balancer-target-groups \
  --auto-scaling-group-name my-asg \
  --target-group-arns arn:aws:elasticloadbalancing:...
```

---

## SQS (Queues)

Managing Simple Queue Service:

| Command | Meaning |
|---------|---------|
| `aws sqs list-queues` | list all queues |
| `aws sqs create-queue` | create new queue |
| `aws sqs send-message` | send message to queue |
| `aws sqs receive-message` | receive messages from queue |
| `aws sqs delete-message` | delete message from queue |
| `aws sqs purge-queue` | delete all messages |

**SQS examples:**
```bash
# Queues
aws sqs list-queues
aws sqs create-queue --queue-name my-queue
aws sqs delete-queue --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/my-queue

# Send messages
aws sqs send-message \
  --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/my-queue \
  --message-body "Hello, World!"

aws sqs send-message \
  --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/my-queue \
  --message-body "Important message" \
  --message-attributes '{"Priority":{"StringValue":"High","DataType":"String"}}'

# Receive messages
aws sqs receive-message --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/my-queue
aws sqs receive-message \
  --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/my-queue \
  --max-number-of-messages 10 \
  --wait-time-seconds 20

# Delete message
aws sqs delete-message \
  --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/my-queue \
  --receipt-handle AQEBwJnKy...

# Queue attributes
aws sqs get-queue-attributes \
  --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/my-queue \
  --attribute-names All

# Purge queue
aws sqs purge-queue --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/my-queue
```

---

## SNS (Notifications)

Managing Simple Notification Service:

| Command | Meaning |
|---------|---------|
| `aws sns list-topics` | list all topics |
| `aws sns create-topic` | create new topic |
| `aws sns publish` | send notification |
| `aws sns subscribe` | subscribe to topic |
| `aws sns list-subscriptions` | list subscriptions |

**SNS examples:**
```bash
# Topics
aws sns list-topics
aws sns create-topic --name my-topic
aws sns delete-topic --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic

# Subscriptions
aws sns subscribe \
  --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic \
  --protocol email \
  --notification-endpoint user@example.com

aws sns subscribe \
  --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic \
  --protocol sms \
  --notification-endpoint +1234567890

aws sns list-subscriptions
aws sns unsubscribe --subscription-arn arn:aws:sns:...

# Publish messages
aws sns publish \
  --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic \
  --message "Hello, World!"

aws sns publish \
  --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic \
  --message "Alert: High CPU usage detected" \
  --subject "System Alert"

# Topic attributes
aws sns get-topic-attributes --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic
aws sns set-topic-attributes \
  --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic \
  --attribute-name DisplayName \
  --attribute-value "My Notification Topic"
```

---

## Common Patterns

Frequently used AWS CLI patterns and workflows:

### Resource Tagging
```bash
# Tag EC2 instances
aws ec2 create-tags \
  --resources i-1234567890abcdef0 \
  --tags Key=Environment,Value=prod Key=Owner,Value=team-alpha

# Tag S3 bucket
aws s3api put-bucket-tagging \
  --bucket my-bucket \
  --tagging 'TagSet=[{Key=Environment,Value=prod},{Key=Project,Value=myapp}]'

# Find resources by tags
aws ec2 describe-instances \
  --filters "Name=tag:Environment,Values=prod" \
  --query 'Reservations[*].Instances[*].[InstanceId,Tags[?Key==`Name`].Value|[0]]' \
  --output table

# Get all resources with specific tag
aws resourcegroupstaggingapi get-resources \
  --tag-filters Key=Environment,Values=prod
```

### Cost and Billing
```bash
# Get billing information
aws ce get-cost-and-usage \
  --time-period Start=2023-07-01,End=2023-07-31 \
  --granularity MONTHLY \
  --metrics BlendedCost

# Get service costs
aws ce get-dimension-values \
  --dimension SERVICE \
  --time-period Start=2023-07-01,End=2023-07-31

# Set up budget
aws budgets create-budget \
  --account-id 123456789012 \
  --budget file://budget.json
```

### Cleanup and Maintenance
```bash
# Find unused resources
aws ec2 describe-volumes --filters Name=status,Values=available  # unattached volumes
aws ec2 describe-addresses --filters Name=association-id,Values=""  # unassociated Elastic IPs
aws ec2 describe-snapshots --owner-ids self --query 'Snapshots[?StartTime<=`2023-01-01`]'  # old snapshots

# Bulk operations
aws ec2 describe-instances \
  --filters "Name=tag:Environment,Values=dev" \
  --query 'Reservations[*].Instances[*].InstanceId' \
  --output text | xargs -n1 aws ec2 terminate-instances --instance-ids

# Delete old AMIs
aws ec2 describe-images --owners self \
  --query 'Images[?CreationDate<=`2022-01-01`].ImageId' \
  --output text | xargs -n1 aws ec2 deregister-image --image-id
```

### Security Auditing
```bash
# Check security groups with open access
aws ec2 describe-security-groups \
  --query 'SecurityGroups[?IpPermissions[?FromPort==`22` && IpRanges[?CidrIp==`0.0.0.0/0`]]].[GroupId,GroupName]' \
  --output table

# Find public S3 buckets
aws s3api list-buckets --query 'Buckets[*].Name' --output text | \
xargs -I {} aws s3api get-bucket-acl --bucket {} --query 'Grants[?Grantee.URI==`http://acs.amazonaws.com/groups/global/AllUsers`]'

# Check IAM users without MFA
aws iam list-users --query 'Users[*].UserName' --output text | \
xargs -I {} aws iam list-mfa-devices --user-name {} --query 'length(MFADevices)'

# Password policy compliance
aws iam get-account-password-policy
aws iam list-users --query 'Users[?PasswordLastUsed<=`2023-01-01`].[UserName,PasswordLastUsed]' --output table
```

### Backup and Disaster Recovery
```bash
# Create EBS snapshots
aws ec2 describe-volumes --query 'Volumes[*].VolumeId' --output text | \
xargs -I {} aws ec2 create-snapshot --volume-id {} --description "Automated backup $(date)"

# RDS snapshots
aws rds describe-db-instances --query 'DBInstances[*].DBInstanceIdentifier' --output text | \
xargs -I {} aws rds create-db-snapshot --db-instance-identifier {} --db-snapshot-identifier {}-backup-$(date +%Y%m%d)

# Cross-region backup
aws s3 sync s3://my-bucket s3://my-backup-bucket --region us-east-1
```

### Automation Scripts
```bash
# Auto-start/stop instances based on tags
#!/bin/bash
# Start instances tagged with AutoStart=true
aws ec2 describe-instances \
  --filters "Name=tag:AutoStart,Values=true" "Name=instance-state-name,Values=stopped" \
  --query 'Reservations[*].Instances[*].InstanceId' \
  --output text | xargs -r aws ec2 start-instances --instance-ids

# Scale down development environments after hours
#!/bin/bash
if [ $(date +%H) -gt 18 ]; then
  aws autoscaling update-auto-scaling-group \
    --auto-scaling-group-name dev-asg \
    --desired-capacity 0
fi

# Automated certificate renewal check
aws acm list-certificates \
  --query 'CertificateSummaryList[?not_after<=`2023-08-01`].[CertificateArn,DomainName]' \
  --output table
```

### Multi-Account Management
```bash
# Switch between accounts using profiles
aws configure list-profiles
export AWS_PROFILE=production
aws sts get-caller-identity

# Cross-account role assumption
aws sts assume-role \
  --role-arn arn:aws:iam::ACCOUNT-B:role/CrossAccountRole \
  --role-session-name cross-account-session \
  --profile account-a

# Assume role and set temporary credentials
TEMP_ROLE=$(aws sts assume-role \
  --role-arn arn:aws:iam::123456789012:role/MyRole \
  --role-session-name temp-session \
  --output json)

export AWS_ACCESS_KEY_ID=$(echo $TEMP_ROLE | jq -r .Credentials.AccessKeyId)
export AWS_SECRET_ACCESS_KEY=$(echo $TEMP_ROLE | jq -r .Credentials.SecretAccessKey)
export AWS_SESSION_TOKEN=$(echo $TEMP_ROLE | jq -r .Credentials.SessionToken)
```

---

## Troubleshooting

Common problems and solutions:

### Authentication Issues
```bash
# Check current credentials
aws sts get-caller-identity
aws configure list

# Invalid credentials
aws configure                    # reconfigure
aws configure --profile myprofile  # use different profile

# Token expired (when using temporary credentials)
aws sts get-session-token        # get new token
unset AWS_SESSION_TOKEN          # clear expired token

# Permission denied
aws iam simulate-principal-policy \
  --policy-source-arn arn:aws:iam::123456789012:user/myuser \
  --action-names s3:GetObject \
  --resource-arns arn:aws:s3:::mybucket/*
```

### Region Issues
```bash
# Resource not found (wrong region)
aws configure get region         # check current region
aws ec2 describe-regions         # list all regions
aws ec2 describe-instances --region us-east-1  # specify region

# Set region for session
export AWS_DEFAULT_REGION=us-west-2
aws configure set region us-west-2
```

### Output and Formatting Issues
```bash
# Disable pager
export AWS_PAGER=""
aws configure set cli_pager ""

# Different output formats
aws ec2 describe-instances --output table
aws ec2 describe-instances --output text
aws ec2 describe-instances --output yaml

# Query specific fields
aws ec2 describe-instances \
  --query 'Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress]' \
  --output table
```

### Performance and Limits
```bash
# Pagination for large result sets
aws s3api list-objects-v2 --bucket large-bucket --max-items 1000
aws ec2 describe-instances --max-items 50

# Retry configuration
aws configure set max_attempts 10
aws configure set retry_mode adaptive

# Check service limits
aws service-quotas list-service-quotas --service-code ec2
aws support describe-trusted-advisor-checks  # requires support plan
```

---

## Advanced Usage

Power user techniques:

### JQ Integration
```bash
# Complex JSON parsing
aws ec2 describe-instances | jq '.Reservations[].Instances[] | select(.State.Name=="running") | .InstanceId'

# Filter by multiple conditions
aws ec2 describe-instances | jq '.Reservations[].Instances[] | select(.Tags[]?.Value=="production" and .State.Name=="running")'

# Create custom output
aws ec2 describe-instances | jq -r '.Reservations[].Instances[] | "\(.InstanceId) \(.InstanceType) \(.State.Name)"'

# Group by tag
aws ec2 describe-instances | jq 'group_by(.Reservations[].Instances[].Tags[]? | select(.Key=="Environment") | .Value)'
```

### Bash Automation
```bash
# Function to get instance IP
get_instance_ip() {
  aws ec2 describe-instances \
    --instance-ids $1 \
    --query 'Reservations[0].Instances[0].PublicIpAddress' \
    --output text
}

# Wait for instance to be running
wait_for_instance() {
  echo "Waiting for instance $1 to be running..."
  aws ec2 wait instance-running --instance-ids $1
  echo "Instance $1 is now running"
}

# Bulk resource creation
create_environments() {
  for env in dev staging prod; do
    aws ec2 run-instances \
      --image-id ami-12345678 \
      --count 1 \
      --instance-type t2.micro \
      --tag-specifications "ResourceType=instance,Tags=[{Key=Environment,Value=$env}]"
  done
}
```

### CLI Configuration
```bash
# Advanced configuration
aws configure set default.s3.max_concurrent_requests 20
aws configure set default.s3.max_bandwidth 100MB/s
aws configure set default.s3.use_accelerate_endpoint true

# Profile-specific settings
aws configure set region us-west-2 --profile prod
aws configure set output json --profile prod

# Environment variables
export AWS_CONFIG_FILE=~/.aws/config-custom
export AWS_SHARED_CREDENTIALS_FILE=~/.aws/credentials-custom
export AWS_DEFAULT_OUTPUT=table
export AWS_DEFAULT_REGION=us-west-2
```

---

## How to Use This Dictionary

### Quick Search

Use Ctrl+F (or Cmd+F on Mac) to quickly find any command or service.

### Learning Path

1. **Start with setup**: configure credentials and regions
2. **Learn core services**: EC2, S3, IAM
3. **Explore networking**: VPC, security groups
4. **Advanced services**: Lambda, ECS, CloudFormation
5. **Automation**: combine commands, use scripts

### Daily Workflow

```bash
# Morning routine
aws sts get-caller-identity      # confirm identity
aws ec2 describe-instances \
  --filters "Name=tag:Team,Values=myteam" \
  --query 'Reservations[*].Instances[*].[InstanceId,State.Name]' \
  --output table                 # check team resources

# Deployment workflow
aws s3 sync ./dist s3://my-website-bucket --delete
aws cloudfront create-invalidation --distribution-id E123456789 --paths "/*"
aws lambda update-function-code --function-name my-api --zip-file fileb://function.zip

# Monitoring workflow
aws logs filter-log-events \
  --log-group-name /aws/lambda/my-function \
  --start-time $(date -d '1 hour ago' +%s)000 \
  --filter-pattern "ERROR"
```

### Best Practices

- **Use profiles** for different environments/accounts
- **Tag everything** for better organization and cost tracking
- **Script repetitive tasks** to avoid mistakes
- **Use IAM roles** instead of long-term access keys when possible
- **Enable CloudTrail** for audit logging
- **Set up billing alerts** to monitor costs

---

## License

This dictionary is open source and can be used freely in your projects and studies.

---

**Last updated:** July 2025  
**Version:** 1.0

*"AWS CLI is powerful but vast. This dictionary helps you navigate the cloud with confidence!"*