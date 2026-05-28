# Terraform AWS Networking Module

## Usage

```hcl
module "networking" {
  source = "git::https://github.com/cgdent-hashicorp/terraform-aws-networking.git?ref=v1.0.0"

  vpc_cidr = "10.0.0.0/16"
}
```
