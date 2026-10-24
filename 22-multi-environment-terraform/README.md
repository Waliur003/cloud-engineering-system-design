# Multi-Environment Terraform Architecture

## System Design Question

Design a Terraform repository that supports **development, staging, and production environments** while keeping infrastructure code reusable and Terraform state isolated between environments.

---

## Architecture Overview

This architecture separates Terraform into two main areas:

- **Reusable modules** contain infrastructure components.
- **Environment folders** contain environment-specific configuration.

Development, staging, and production reuse the same Terraform modules but provide different variable values and maintain separate Terraform state.

---

## High-Level Architecture

```text
                     Terraform Repository
                            |
            +---------------+---------------+
            |                               |
            v                               v
         modules/                      environments/
            |                               |
     +------+------+                 +------+------+ 
     |      |      |                 |      |      |
     v      v      v                 v      v      v
    VPC    EC2    RDS               DEV  STAGING  PROD
     |      |      |                 |      |      |
     +------+------+-----------------+------+------+ 
                                            |
                                            v
                                    Remote Backend
                                            |
                                            v
                                       Amazon S3
                                    /       |       \
                                   /        |        \
                            dev.tfstate staging.tfstate prod.tfstate
```

---

## Repository Structure

```text
terraform/
│
├── modules/
│   │
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   ├── ec2/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   ├── alb/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   └── rds/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
│
└── environments/
    │
    ├── dev/
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── terraform.tfvars
    │   └── backend.tf
    │
    ├── staging/
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── terraform.tfvars
    │   └── backend.tf
    │
    └── prod/
        ├── main.tf
        ├── variables.tf
        ├── terraform.tfvars
        └── backend.tf
```

---

## Reusable Modules

Terraform modules contain reusable infrastructure code.

For example:

```text
modules/
├── vpc/
├── ec2/
├── alb/
└── rds/
```

The same modules can be called by all environments.

```text
                    Shared Modules
                         |
             +-----------+-----------+
             |           |           |
             v           v           v
            DEV       STAGING       PROD
```

This avoids copying the same VPC, EC2, RDS, or ALB configuration multiple times.

---

## Environment Architecture

Each environment calls the shared modules independently.

```text
                         modules/
                            |
            +---------------+---------------+
            |               |               |
            v               v               v
           DEV           STAGING           PROD
            |               |               |
      dev.tfvars      staging.tfvars    prod.tfvars
            |               |               |
            v               v               v
     Dev Resources    Staging Resources  Prod Resources
```

The infrastructure structure can remain the same while capacity and configuration differ.

---

## Environment Variables

Each environment can provide different values to the same module.

### Development

```hcl
environment   = "dev"
instance_type = "t3.micro"
min_size      = 1
max_size      = 2
```

### Staging

```hcl
environment   = "staging"
instance_type = "t3.small"
min_size      = 2
max_size      = 4
```

### Production

```hcl
environment   = "prod"
instance_type = "m6i.large"
min_size      = 4
max_size      = 10
```

The pattern is:

```text
Same Terraform Module
        +
Different Variables
        =
Different Environment
```

---

## Module Usage

An environment can call reusable modules from its `main.tf`.

```hcl
module "vpc" {
  source = "../../modules/vpc"

  environment = var.environment
  vpc_cidr    = var.vpc_cidr
}

module "ec2" {
  source = "../../modules/ec2"

  environment   = var.environment
  instance_type = var.instance_type
}

module "rds" {
  source = "../../modules/rds"

  environment = var.environment
}
```

The module code remains reusable while each environment supplies its own configuration.

---

## Remote State Architecture

Each environment stores Terraform state remotely.

```text
DEV
 |
 +-----> Remote Backend
              |
              +-----> dev.tfstate


STAGING
 |
 +-----> Remote Backend
              |
              +-----> staging.tfstate


PROD
 |
 +-----> Remote Backend
              |
              +-----> prod.tfstate
```

A development deployment should never modify the production state.

---

## Amazon S3 Backend

Terraform state can be stored in Amazon S3.

Example structure:

```text
terraform-state-bucket/
│
├── dev/
│   └── terraform.tfstate
│
├── staging/
│   └── terraform.tfstate
│
└── prod/
    └── terraform.tfstate
```

This keeps environment state isolated.

---

## Example Backend Configuration

Development:

```hcl
terraform {
  backend "s3" {
    bucket       = "company-terraform-state"
    key          = "dev/terraform.tfstate"
    region       = "us-east-1"
    use_lockfile = true
    encrypt      = true
  }
}
```

Production can use a different state key:

```hcl
terraform {
  backend "s3" {
    bucket       = "company-terraform-state"
    key          = "prod/terraform.tfstate"
    region       = "us-east-1"
    use_lockfile = true
    encrypt      = true
  }
}
```

---

## State Isolation

State separation is one of the most important parts of the architecture.

```text
                  Amazon S3 Backend
                        |
         +--------------+--------------+
         |              |              |
         v              v              v
    DEV STATE      STAGING STATE    PROD STATE
```

Therefore:

```text
terraform destroy
inside dev/
      ↓
DEV resources only
```

It should not destroy staging or production resources.

---

## State Locking

State locking prevents multiple Terraform operations from modifying the same state simultaneously.

```text
Engineer / Pipeline A ----\
                           \
                            ---> State Lock ---> Terraform State
                           /
Engineer / Pipeline B ----/
```

If Pipeline A holds the lock, Pipeline B must wait until the state becomes available.

This helps prevent state corruption.

---

## Environment Isolation

The environments should behave independently.

```text
DEV
 ├── Configuration
 ├── Variables
 ├── Resources
 └── State

STAGING
 ├── Configuration
 ├── Variables
 ├── Resources
 └── State

PROD
 ├── Configuration
 ├── Variables
 ├── Resources
 └── State
```

A change in one environment should not automatically affect another environment.

---

## Deployment Example

A developer working on development infrastructure might run:

```bash
cd environments/dev

terraform init
terraform plan
terraform apply
```

For production:

```bash
cd environments/prod

terraform init
terraform plan
terraform apply
```

Each directory uses its own environment configuration and state.

---

## Why Use Modules?

Without modules:

```text
dev/
  duplicated VPC code
  duplicated EC2 code
  duplicated RDS code

staging/
  duplicated VPC code
  duplicated EC2 code
  duplicated RDS code

prod/
  duplicated VPC code
  duplicated EC2 code
  duplicated RDS code
```

This creates unnecessary duplication.

With modules:

```text
                modules/
                   |
        +----------+----------+
        |          |          |
       DEV      STAGING      PROD
```

Infrastructure code is written once and reused.

---

## Security and Safety

A production-friendly Terraform architecture should include:

- Separate state per environment
- Remote encrypted state
- State locking
- Least-privilege AWS permissions
- Production approval controls
- Protected Git branches
- State bucket versioning
- Limited access to production configuration

Production state should be treated as sensitive because Terraform state may contain infrastructure details and potentially sensitive values.

---

## Key Design Decisions

| Design | Purpose |
|---|---|
| `modules/` | Stores reusable infrastructure code |
| `environments/` | Stores environment-specific configuration |
| `terraform.tfvars` | Supplies environment-specific values |
| `backend.tf` | Defines remote state configuration |
| Separate state | Prevents one environment from affecting another |
| Amazon S3 | Stores remote Terraform state |
| State locking | Prevents concurrent state modification |
| Variables | Allow different sizing and configuration |
| Modules | Prevent duplicated Terraform code |

---

## Key Architecture Pattern

```text
                   REUSABLE CODE
                       modules/
                          |
          +---------------+---------------+
          |               |               |
          v               v               v
         DEV           STAGING           PROD
          |               |               |
    dev.tfvars      staging.tfvars    prod.tfvars
          |               |               |
          v               v               v
    Dev Resources   Staging Resources Prod Resources
          |               |               |
          v               v               v
      Dev State       Staging State     Prod State
```

---

## Key Takeaway

A clean multi-environment Terraform architecture follows:

```text
Reusable Modules
       +
Environment-Specific Variables
       +
Separate Environment Folders
       +
Separate Remote State
       +
State Locking
       =
Safe Multi-Environment Infrastructure
```

The most important rule is:

> **Reuse the infrastructure code, but isolate the environment configuration and Terraform state.**