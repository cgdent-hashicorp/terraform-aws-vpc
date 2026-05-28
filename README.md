# Terraform AWS VPC Module

Terraform module for provisioning a reusable AWS VPC foundation including networking resources such as:

* VPC
* Public subnets
* Internet Gateway
* Route tables
* Route table associations
* Tags and naming conventions

This module is designed for use with HCP Terraform and reusable enterprise infrastructure patterns.

---

# Architecture

This module provisions:

* 1 VPC
* Public subnet
* Internet Gateway
* Public route table
* Route table associations

Example architecture:

```text
Internet Gateway
        │
        ▼
+----------------------+
|        VPC           |
|  10.0.0.0/16         |
|                      |
|  +----------------+  |
|  | Public Subnet  |  |
|  | 10.0.1.0/24    |  |
|  +----------------+  |
|                      |
+----------------------+
```

---

# Requirements

| Name         | Version  |
| ------------ | -------- |
| Terraform    | >= 1.5.0 |
| AWS Provider | ~> 5.0   |

---

# Usage

```hcl
module "vpc" {
  source  = "app.terraform.io/<ORG>/vpc/aws"
  version = "1.0.0"

  name               = "acme-demo"
  vpc_cidr           = "10.0.0.0/16"
  public_subnet_cidr = "10.0.1.0/24"

  tags = {
    Environment = "dev"
    Owner       = "platform-team"
    Terraform   = "true"
  }
}
```

---

# Providers

```hcl
provider "aws" {
  region = "ap-southeast-2"
}
```

---

# Inputs

| Name               | Description                      | Type        | Required |
| ------------------ | -------------------------------- | ----------- | -------- |
| name               | Name prefix for resources        | string      | yes      |
| vpc_cidr           | CIDR block for the VPC           | string      | yes      |
| public_subnet_cidr | CIDR block for the public subnet | string      | yes      |
| tags               | Tags applied to resources        | map(string) | no       |

---

# Outputs

| Name                | Description                |
| ------------------- | -------------------------- |
| vpc_id              | ID of the VPC              |
| public_subnet_id    | ID of the public subnet    |
| internet_gateway_id | ID of the Internet Gateway |

---

# Example

Example Terraform root module:

```hcl
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-southeast-2"
}

module "vpc" {
  source  = "app.terraform.io/acme-platform/vpc/aws"
  version = "1.0.0"

  name               = "demo"
  vpc_cidr           = "10.0.0.0/16"
  public_subnet_cidr = "10.0.1.0/24"

  tags = {
    Environment = "dev"
  }
}
```

---

# Versioning

This module follows Semantic Versioning.

| Version | Description                      |
| ------- | -------------------------------- |
| v1.x.x  | Backward-compatible improvements |
| v2.x.x  | Breaking changes                 |

---

# Best Practices

* Pin module versions in production
* Use separate environments/workspaces
* Avoid modifying resources manually outside Terraform
* Store Terraform state remotely
* Use least-privilege IAM permissions

---

# Module Development

To create a new release:

1. Push changes to GitHub
2. Create a new Git tag/release:

```bash
git tag v1.1.0
git push origin v1.1.0
```

3. HCP Terraform will automatically detect the new module version.

---

# License

MIT License
