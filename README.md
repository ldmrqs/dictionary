# Terraform Dictionary

A complete reference of Terraform commands and concepts in a direct and straightforward way.

## Table of Contents

- [Basic Commands](#basic-commands)
- [Core Concepts](#core-concepts)
- [Configuration Language (HCL)](#configuration-language-hcl)
- [Resource Management](#resource-management)
- [Variables and Outputs](#variables-and-outputs)
- [Modules](#modules)
- [State Management](#state-management)
- [Providers](#providers)
- [Workspaces](#workspaces)
- [Data Sources](#data-sources)
- [Built-in Functions](#built-in-functions)
- [Provisioners](#provisioners)
- [Import and Migration](#import-and-migration)
- [Best Practices](#best-practices)
- [Common Patterns](#common-patterns)

---

## Basic Commands

Essential Terraform commands you'll use every day:

| Command | Meaning |
|---------|---------|
| `terraform init` | initialize working directory |
| `terraform plan` | show what will be created/changed |
| `terraform apply` | create/update infrastructure |
| `terraform destroy` | destroy all managed infrastructure |
| `terraform validate` | check configuration syntax |
| `terraform fmt` | format configuration files |
| `terraform show` | show current state |
| `terraform version` | show Terraform version |

**Example:**
```bash
terraform init                   # initialize project
terraform plan                   # preview changes
terraform apply                  # create infrastructure
terraform show                   # see current state
terraform destroy                # tear everything down
```

---

## Core Concepts

Fundamental Terraform concepts:

| Concept | Meaning |
|---------|---------|
| **Resource** | infrastructure component (VM, database, etc.) |
| **Provider** | plugin that manages specific platform (AWS, Azure) |
| **State** | current status of your infrastructure |
| **Configuration** | .tf files that describe desired infrastructure |
| **Module** | reusable group of resources |
| **Backend** | where state is stored |
| **Workspace** | isolated state environments |
| **Data Source** | read-only information from provider |

**Example:**
```hcl
# Provider - connects to AWS
provider "aws" {
  region = "us-west-2"
}

# Resource - creates EC2 instance
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer"
  }
}

# Data Source - gets existing VPC info
data "aws_vpc" "default" {
  default = true
}
```

---

## Configuration Language (HCL)

Terraform configuration syntax:

| Syntax | Meaning |
|--------|---------|
| `resource "type" "name" {}` | define resource |
| `variable "name" {}` | define input variable |
| `output "name" {}` | define output value |
| `locals {}` | define local values |
| `module "name" {}` | call module |
| `data "type" "name" {}` | define data source |
| `provider "name" {}` | configure provider |
| `terraform {}` | terraform settings |

**Basic syntax examples:**
```hcl
# Variables
variable "instance_count" {
  description = "Number of instances"
  type        = number
  default     = 1
}

variable "environment" {
  description = "Environment name"
  type        = string
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

# Locals
locals {
  common_tags = {
    Environment = var.environment
    Project     = "MyApp"
  }
}

# Outputs
output "instance_ip" {
  description = "IP address of instance"
  value       = aws_instance.web.public_ip
}
```

---

## Resource Management

Working with infrastructure resources:

| Pattern | Meaning |
|---------|---------|
| `resource "aws_instance" "web"` | create EC2 instance |
| `count = 3` | create multiple resources |
| `for_each = toset(["a", "b"])` | create resources for each item |
| `depends_on = [aws_vpc.main]` | explicit dependency |
| `lifecycle { prevent_destroy = true }` | prevent accidental deletion |
| `tags = local.common_tags` | apply tags |

**Examples:**
```hcl
# Single resource
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer"
  }
}

# Multiple resources with count
resource "aws_instance" "workers" {
  count         = var.instance_count
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  tags = {
    Name = "Worker-${count.index + 1}"
  }
}

# Multiple resources with for_each
resource "aws_instance" "apps" {
  for_each = toset(["web", "api", "worker"])
  
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  tags = {
    Name = "App-${each.key}"
    Type = each.value
  }
}

# Resource with lifecycle rules
resource "aws_s3_bucket" "data" {
  bucket = "my-important-data"
  
  lifecycle {
    prevent_destroy = true
    ignore_changes  = [tags]
  }
}
```

---

## Variables and Outputs

Managing inputs and outputs:

| Type | Meaning |
|------|---------|
| `string` | text value |
| `number` | numeric value |
| `bool` | true/false value |
| `list(string)` | list of strings |
| `map(string)` | key-value pairs |
| `object({})` | complex object with typed attributes |
| `any` | any type (avoid when possible) |

**Variable examples:**
```hcl
# Simple variables
variable "region" {
  description = "AWS region"
  type        = string
  default     = "us-west-2"
}

variable "instance_count" {
  description = "Number of instances"
  type        = number
  default     = 2
}

variable "enable_monitoring" {
  description = "Enable monitoring"
  type        = bool
  default     = true
}

# Complex variables
variable "environments" {
  description = "Environment configurations"
  type = map(object({
    instance_type = string
    min_size     = number
    max_size     = number
  }))
  default = {
    dev = {
      instance_type = "t2.micro"
      min_size     = 1
      max_size     = 2
    }
    prod = {
      instance_type = "t3.medium"
      min_size     = 2
      max_size     = 10
    }
  }
}

# Using variables
resource "aws_instance" "app" {
  count         = var.instance_count
  ami           = "ami-12345678"
  instance_type = var.environments[var.environment].instance_type
  
  monitoring = var.enable_monitoring
}

# Outputs
output "instance_ips" {
  description = "IP addresses of instances"
  value       = aws_instance.app[*].public_ip
}

output "load_balancer_dns" {
  description = "Load balancer DNS name"
  value       = aws_lb.main.dns_name
  sensitive   = false
}
```

---

## Modules

Creating and using reusable components:

| Pattern | Meaning |
|---------|---------|
| `module "name" { source = "./modules/vpc" }` | use local module |
| `module "name" { source = "git::https://..." }` | use git module |
| `module "name" { source = "registry..." }` | use registry module |
| `module.vpc.vpc_id` | reference module output |

**Module structure:**
```
modules/
├── vpc/
│   ├── main.tf       # resources
│   ├── variables.tf  # inputs
│   ├── outputs.tf    # outputs
│   └── versions.tf   # provider requirements
```

**Module example:**
```hcl
# modules/vpc/variables.tf
variable "cidr_block" {
  description = "CIDR block for VPC"
  type        = string
  default     = "10.0.0.0/16"
}

variable "availability_zones" {
  description = "AZs for subnets"
  type        = list(string)
}

# modules/vpc/main.tf
resource "aws_vpc" "main" {
  cidr_block           = var.cidr_block
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name = "main-vpc"
  }
}

resource "aws_subnet" "public" {
  count             = length(var.availability_zones)
  vpc_id            = aws_vpc.main.id
  cidr_block        = cidrsubnet(var.cidr_block, 8, count.index)
  availability_zone = var.availability_zones[count.index]
  
  map_public_ip_on_launch = true
  
  tags = {
    Name = "public-subnet-${count.index + 1}"
  }
}

# modules/vpc/outputs.tf
output "vpc_id" {
  description = "VPC ID"
  value       = aws_vpc.main.id
}

output "public_subnet_ids" {
  description = "Public subnet IDs"
  value       = aws_subnet.public[*].id
}

# Using the module
module "vpc" {
  source = "./modules/vpc"
  
  cidr_block         = "10.0.0.0/16"
  availability_zones = ["us-west-2a", "us-west-2b"]
}

resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  subnet_id     = module.vpc.public_subnet_ids[0]
}
```

---

## State Management

Managing Terraform state:

| Command | Meaning |
|---------|---------|
| `terraform state list` | list resources in state |
| `terraform state show <resource>` | show resource details |
| `terraform state mv <old> <new>` | rename resource in state |
| `terraform state rm <resource>` | remove resource from state |
| `terraform state pull` | download remote state |
| `terraform state push` | upload state to remote |
| `terraform refresh` | update state from real infrastructure |

**Backend configuration:**
```hcl
# S3 backend for remote state
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-west-2"
    
    # Optional: DynamoDB for state locking
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}

# Alternative: Terraform Cloud backend
terraform {
  cloud {
    organization = "my-org"
    
    workspaces {
      name = "my-app-prod"
    }
  }
}
```

**State commands examples:**
```bash
terraform state list                    # see all resources
terraform state show aws_instance.web   # detailed resource info
terraform state mv aws_instance.web aws_instance.app  # rename
terraform state rm aws_instance.old     # remove from state
terraform import aws_instance.web i-1234567890abcdef0  # import existing
```

---

## Providers

Working with different cloud platforms:

| Provider | Usage |
|----------|-------|
| `aws` | Amazon Web Services |
| `azurerm` | Microsoft Azure |
| `google` | Google Cloud Platform |
| `kubernetes` | Kubernetes clusters |
| `helm` | Helm charts |
| `random` | generate random values |
| `local` | local files and commands |
| `null` | dummy provider for provisioners |

**Provider examples:**
```hcl
# AWS Provider
provider "aws" {
  region = var.aws_region
  
  default_tags {
    tags = {
      Environment = var.environment
      Project     = "MyApp"
    }
  }
}

# Azure Provider
provider "azurerm" {
  features {}
  subscription_id = var.azure_subscription_id
}

# Google Cloud Provider
provider "google" {
  project = var.gcp_project
  region  = var.gcp_region
}

# Multiple provider instances
provider "aws" {
  alias  = "us_east"
  region = "us-east-1"
}

provider "aws" {
  alias  = "us_west"
  region = "us-west-2"
}

# Using aliased providers
resource "aws_instance" "east" {
  provider = aws.us_east
  ami      = "ami-12345678"
}

resource "aws_instance" "west" {
  provider = aws.us_west
  ami      = "ami-87654321"
}
```

---

## Workspaces

Managing multiple environments:

| Command | Meaning |
|---------|---------|
| `terraform workspace list` | list all workspaces |
| `terraform workspace show` | show current workspace |
| `terraform workspace new <name>` | create new workspace |
| `terraform workspace select <name>` | switch to workspace |
| `terraform workspace delete <name>` | delete workspace |

**Using workspaces:**
```bash
# Create environments
terraform workspace new dev
terraform workspace new staging  
terraform workspace new prod

# Switch between environments
terraform workspace select dev
terraform apply

terraform workspace select prod
terraform apply
```

**Workspace-aware configuration:**
```hcl
# Different settings per workspace
locals {
  environment_configs = {
    dev = {
      instance_type = "t2.micro"
      instance_count = 1
    }
    staging = {
      instance_type = "t2.small"
      instance_count = 2
    }
    prod = {
      instance_type = "t3.medium"
      instance_count = 5
    }
  }
  
  current_config = local.environment_configs[terraform.workspace]
}

resource "aws_instance" "app" {
  count         = local.current_config.instance_count
  instance_type = local.current_config.instance_type
  ami           = "ami-12345678"
  
  tags = {
    Name        = "app-${terraform.workspace}-${count.index + 1}"
    Environment = terraform.workspace
  }
}
```

---

## Data Sources

Reading existing infrastructure:

| Data Source | Usage |
|-------------|-------|
| `data "aws_ami" "latest"` | get latest AMI |
| `data "aws_vpc" "default"` | get default VPC |
| `data "aws_availability_zones" "available"` | list AZs |
| `data "aws_caller_identity" "current"` | current AWS account |
| `data "local_file" "config"` | read local file |

**Data source examples:**
```hcl
# Get latest Amazon Linux AMI
data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]
  
  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}

# Get default VPC
data "aws_vpc" "default" {
  default = true
}

# Get available AZs
data "aws_availability_zones" "available" {
  state = "available"
}

# Get current AWS account info
data "aws_caller_identity" "current" {}

# Use data sources
resource "aws_instance" "web" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = "t2.micro"
  subnet_id     = data.aws_vpc.default.id
  
  tags = {
    Name      = "web-server"
    AccountId = data.aws_caller_identity.current.account_id
  }
}

output "available_zones" {
  value = data.aws_availability_zones.available.names
}
```

---

## Built-in Functions

Useful Terraform functions:

| Function | Usage |
|----------|-------|
| `length(list)` | get list/map length |
| `join(",", list)` | join list elements |
| `split(",", string)` | split string into list |
| `upper(string)` | convert to uppercase |
| `lower(string)` | convert to lowercase |
| `cidrsubnet(cidr, bits, index)` | calculate subnet CIDR |
| `file(path)` | read file contents |
| `base64encode(string)` | encode to base64 |
| `jsonencode(object)` | convert to JSON |
| `toset(list)` | convert list to set |

**Function examples:**
```hcl
locals {
  # String functions
  app_name_upper = upper(var.app_name)
  environment    = lower(var.environment)
  
  # List functions
  az_count = length(data.aws_availability_zones.available.names)
  subnet_cidrs = [
    for i in range(local.az_count) : 
    cidrsubnet(var.vpc_cidr, 8, i)
  ]
  
  # File and encoding
  user_data = base64encode(file("${path.module}/user-data.sh"))
  
  # Object manipulation
  tags = merge(var.common_tags, {
    Name = "${var.app_name}-${var.environment}"
  })
}

# Using functions in resources
resource "aws_subnet" "public" {
  count = length(data.aws_availability_zones.available.names)
  
  vpc_id            = aws_vpc.main.id
  cidr_block        = local.subnet_cidrs[count.index]
  availability_zone = data.aws_availability_zones.available.names[count.index]
  
  tags = merge(local.tags, {
    Name = "public-subnet-${count.index + 1}"
  })
}

# Conditional expressions
resource "aws_instance" "web" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = var.environment == "prod" ? "t3.medium" : "t2.micro"
  
  root_block_device {
    volume_size = var.environment == "prod" ? 20 : 8
  }
}
```

---

## Provisioners

Running scripts and commands:

| Provisioner | Usage |
|-------------|-------|
| `local-exec` | run command on local machine |
| `remote-exec` | run command on remote resource |
| `file` | copy files to remote resource |

**Provisioner examples:**
```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  key_name      = var.key_name
  
  # Copy file to instance
  provisioner "file" {
    source      = "app.py"
    destination = "/tmp/app.py"
    
    connection {
      type        = "ssh"
      user        = "ec2-user"
      private_key = file(var.private_key_path)
      host        = self.public_ip
    }
  }
  
  # Run commands on instance
  provisioner "remote-exec" {
    inline = [
      "sudo yum update -y",
      "sudo yum install -y python3",
      "sudo mv /tmp/app.py /opt/app.py",
      "sudo python3 /opt/app.py"
    ]
    
    connection {
      type        = "ssh"
      user        = "ec2-user"
      private_key = file(var.private_key_path)
      host        = self.public_ip
    }
  }
  
  # Run local command after creation
  provisioner "local-exec" {
    command = "echo 'Instance ${self.id} created with IP ${self.public_ip}' >> instances.log"
  }
  
  # Cleanup on destroy
  provisioner "local-exec" {
    when    = destroy
    command = "echo 'Instance destroyed' >> cleanup.log"
  }
}
```

---

## Import and Migration

Bringing existing infrastructure under Terraform management:

| Command | Usage |
|---------|-------|
| `terraform import <resource> <id>` | import existing resource |
| `terraform plan -generate-config-out=generated.tf` | generate config for imported resources |

**Import workflow:**
```bash
# 1. Add resource to configuration (without details)
resource "aws_instance" "existing" {
  # Configuration will be filled after import
}

# 2. Import the resource
terraform import aws_instance.existing i-1234567890abcdef0

# 3. Get current configuration
terraform show -json | jq '.values.root_module.resources[]'

# 4. Update configuration to match current state
resource "aws_instance" "existing" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  # ... other attributes
}

# 5. Plan to see if configuration matches
terraform plan
```

**Migration example:**
```hcl
# Before: manually created resources
# After: bring under Terraform management

# Step 1: Define resources in Terraform
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "main-vpc"
  }
}

resource "aws_subnet" "public" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
  
  tags = {
    Name = "public-subnet"
  }
}

# Step 2: Import existing resources
# terraform import aws_vpc.main vpc-12345678
# terraform import aws_subnet.public subnet-87654321

# Step 3: Adjust configuration to match actual state
# terraform plan (should show no changes)
```

---

## Best Practices

Guidelines for effective Terraform usage:

### File Organization
```
project/
├── main.tf              # main resources
├── variables.tf         # input variables
├── outputs.tf          # output values
├── versions.tf         # provider requirements
├── terraform.tfvars    # variable values (don't commit secrets)
├── modules/            # local modules
└── environments/       # environment-specific configs
    ├── dev/
    ├── staging/
    └── prod/
```

### Naming Conventions
```hcl
# Use descriptive names
resource "aws_instance" "web_server" {}        # Good
resource "aws_instance" "instance" {}          # Bad

# Use consistent naming
resource "aws_security_group" "web_server_sg" {}
resource "aws_launch_template" "web_server_lt" {}

# Environment prefixes
resource "aws_vpc" "main" {
  tags = {
    Name = "${var.environment}-vpc"
  }
}
```

### Security Best Practices
```hcl
# Don't hardcode secrets
variable "database_password" {
  description = "Database password"
  type        = string
  sensitive   = true
}

# Use data sources for sensitive info
data "aws_secretsmanager_secret_version" "db_password" {
  secret_id = "prod/database/password"
}

# Enable encryption
resource "aws_s3_bucket_encryption" "data" {
  bucket = aws_s3_bucket.data.id
  
  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }
}
```

### Version Constraints
```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    random = {
      source  = "hashicorp/random"
      version = "~> 3.1"
    }
  }
}
```

---

## Common Patterns

Frequently used Terraform patterns:

### Multi-Environment Setup
```hcl
# environments/dev/main.tf
module "infrastructure" {
  source = "../../modules/infrastructure"
  
  environment    = "dev"
  instance_type  = "t2.micro"
  instance_count = 1
  
  common_tags = {
    Environment = "dev"
    Project     = "myapp"
  }
}

# environments/prod/main.tf
module "infrastructure" {
  source = "../../modules/infrastructure"
  
  environment    = "prod"
  instance_type  = "t3.large"
  instance_count = 3
  
  common_tags = {
    Environment = "prod"
    Project     = "myapp"
  }
}
```

### Conditional Resources
```hcl
# Create load balancer only in production
resource "aws_lb" "main" {
  count = var.environment == "prod" ? 1 : 0
  
  name               = "main-lb"
  internal           = false
  load_balancer_type = "application"
  subnets            = var.public_subnet_ids
}

# Different configurations per environment
locals {
  instance_config = {
    dev = {
      type  = "t2.micro"
      count = 1
    }
    prod = {
      type  = "t3.large"
      count = 3
    }
  }
}

resource "aws_instance" "app" {
  count         = local.instance_config[var.environment].count
  instance_type = local.instance_config[var.environment].type
  ami           = data.aws_ami.amazon_linux.id
}
```

### Dynamic Blocks
```hcl
resource "aws_security_group" "web" {
  name_prefix = "web-sg"
  vpc_id      = var.vpc_id
  
  # Dynamic ingress rules
  dynamic "ingress" {
    for_each = var.allowed_ports
    content {
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# Usage
module "web_sg" {
  source = "./modules/security-group"
  
  vpc_id        = module.vpc.vpc_id
  allowed_ports = [80, 443, 22]
}
```

---

## Troubleshooting

Common problems and solutions:

### State Issues
```bash
# State is locked
terraform force-unlock LOCK_ID

# State corruption
terraform state pull > backup.tfstate
# Fix manually, then:
terraform state push fixed.tfstate

# Drift detection
terraform plan -refresh-only
terraform apply -refresh-only
```

### Import Problems
```bash
# Resource exists but not in state
terraform import aws_instance.web i-1234567890abcdef0

# Multiple resources
terraform import 'aws_instance.web[0]' i-1234567890abcdef0
terraform import 'aws_instance.web[1]' i-0987654321fedcba0
```

### Plan/Apply Issues
```bash
# Force refresh
terraform apply -refresh=true

# Target specific resource
terraform apply -target=aws_instance.web

# Ignore specific resource
terraform apply -replace=aws_instance.web
```

---

## How to Use This Dictionary

### Quick Search

Use Ctrl+F (or Cmd+F on Mac) to quickly find any command or concept.

### Learning Path

1. **Start with basics**: init, plan, apply, destroy
2. **Learn resources**: create simple infrastructure
3. **Master variables**: make configurations flexible
4. **Explore modules**: create reusable components
5. **Advanced topics**: state management, imports

### Real-World Workflow

```bash
# Daily workflow
terraform init                   # once per project
terraform plan                   # before every apply
terraform apply                  # create/update infrastructure
terraform show                   # verify current state

# Development workflow
terraform workspace new feature-branch
terraform plan
terraform apply
# test changes
terraform destroy               # clean up
terraform workspace select main
```

---

## License

This dictionary is open source and can be used freely in your projects and studies.

---

**Last updated:** July 2025  
**Version:** 1.0

*"Infrastructure as Code doesn't have to be complicated. This dictionary makes Terraform approachable for everyone!"*