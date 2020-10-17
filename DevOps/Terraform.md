<h2>Terraform Crash Course </h2>

- [Overview](#overview)
  - [Project Structure](#project-structure)
- [Configuration Language](#configuration-language)
  - [Basic](#basic)
  - [Blocks](#blocks)
    - [Terraform Setting Block](#terraform-setting-block)
    - [Providers Block](#providers-block)
- [CLI Commands](#cli-commands)
- [Modules](#modules)
- [Input, Output and Local variables](#input-output-and-local-variables)
  - [Basics of Input Variables](#basics-of-input-variables)
  - [Passing Input Variables](#passing-input-variables)
  - [Outputs](#outputs)
  - [Locals](#locals)
- [State, Backend and Data Block](#state-backend-and-data-block)
  - [State](#state)
  - [Backend](#backend)
  - [Data Block](#data-block)
- [Functions](#functions)
- [TerraTest](#terratest)

## Overview

**Naming conventions** terraform variable should use underscore, while azure names should use dash in it.

Terraform consolidate all files in an order and execute them like one big TF file.

[Terraform.gitignore](https://github.com/github/gitignore/blob/master/Terraform.gitignore)

### Project Structure

```
$ tree my-project/
.
├── LICENSE
├── README.md
├── main.tf
├── modules
│   └── aws-s3-static-website-bucket
├── outputs.tf
├── terraform.tfstate
├── terraform.tfstate.backup
└── variables.tf
```

## Configuration Language

### Basic

[**Terraform HCL**](https://www.terraform.io/docs/configuration/index.html)

[**HCL Reference docs**](https://github.com/hashicorp/hcl/blob/hcl2/hclsyntax/spec.md)

**everything a block**

```
<BLOCK TYPE> "<BLOCK LABEL>" "<BLOCK LABEL>" {
  # This is  a comment
  <IDENTIFIER> = <EXPRESSION> # Argument
}
```

example

```
resource "aws_vpc" "main" {
  cidr_block = var.base_cidr_block
}
```

**type of blocks**

- `terraform`
- `provider`
- `resource`
- `module`
- `variable`
- `output`
- `data`

**meta argument** - apply to every resource type, `depends_on`(use in cases where implicit dependency is not identifiable), `count` and `for_each`(create multiple resource),`provisioner` and `connection`(additional action after resource creation), `lifecycle`, `provider`(select non default provider)

- one resource can use either `count` or `for-each`

**Data types in terraform**

- `string`
- `number`
- `bool`
- `object({key1=value1})`
- `list(<TYPE>)`
- `map(<TYPE>)`
- `set(<TYPE>)`
- `any`, i.e. `collection(any)`
- `tuple[Type,Type]`
- `null` - use in expressions

you can use a non-literal expression as a key by wrapping it in parentheses, like `(var.business_unit_tag_name) = "SRE"`.

**multi line syntax** starts with `<<EOF` at the end of a line and ends with `EOF` in its own line

```
resource "aws_s3_bucket" "s3_bucket" {
  policy = <<EOF
    {
    "Version": "2012-10-17",
    "Statement": [
    }
    EOF
```

**override and merge** - in order to override an existing config define new files named as `*_override.tf` or `override.tf`.

> i would recommend to read through https://www.terraform.io/docs/configuration/expressions.html few times, during initial days of TF work

### Blocks

#### Terraform Setting Block

use this to specify global settings

```
terraform {
  # terraform version
  required_version = ">= 0.12.18"

  # define provider version here
  required_providers {
    aws = "~> 1.0"
  }

  # define remote state storage
  backend "s3" {
  }

  # enable experimental features
  experiments = [variable_validation]
}
```

#### Providers Block

- don't use version key in providers block instead use `required_providers` within terraform block
- we can define and use multiple providers block for one provider

Terraform creates and manage a infrastructure object represented by a resource block, the identifier for that real object is saved in Terraform's state. Terraform compares the actual configuration of the object with the arguments given in the configuration before actually applying that on infra

## CLI Commands

- `terrafrom init` - init provider resources, modules, backend
- `terraform plan -refresh=false -var-file=../local.tfvars`
- `terrafrom apply` </br>
  `$ terraform apply -refresh=false -auto-approve=true -var-file=../local.tfvars`
- `terrafrom destroy`
- `terraform get` - get remote module in local module directory `.terraform/modules/`
- **`terrafrom console`** - i.e. test TF functions behavior, expressions
- `terrafrom fmt`
- `terrafrom graph` - generate DOT files that can be visualize using https://graphviz.org/
- `terraform taint` - mark a resource as corrupt, forcing it to be destroyed and recreated.
- `terraform state`
- `terraform refresh`

## Modules

Comparing modules to functions in a traditional programming language. Input, Output and Locals are module scoped

A directory having one or more `.tf` files is a module. When you run `terraform` commands directly from such a directory, it is considered the **root module**.

**Local and remote modules** - Modules can either be loaded from the local filesystem, or a remote source. Terraform supports a variety of sources[(local files, Terraform Registry, version control systems, HTTP URLs) refer **Sources** for more](https://www.terraform.io/docs/modules/sources.html).

**[registry.terraform.io](https://registry.terraform.io/)** - public module registry

**Module Dependencies**

**How to use a module** - using module block

```terraform
module "calling_module_local_name" {
    source  = "git, http, module name on terraform-registry"
    name   = "assets" # use when we need multiple instance of same module
    version = "module_version" # best to use it always

    # optional, defaults to all root module providers
    providers = {
    aws = aws.west
    }

    input_var1 = value1
    input_var2 = value2
}
```

The child module can then use any resource from parent module's providers with no further provider configuration required

> modules sourced from local file paths do not support version

## Input, Output and Local variables

### Basics of Input Variables

**defining variable block**

```
variable "vpc_name" {
  type        = string
  description = "Name of VPC"
  default     = "example-vpc"
  validation {
    condition  = expression
    error_message = ""
  }
}
```

- use variable as `var.variable_block_name` i.e. `var.vpc_name`

### Passing Input Variables

- pass a file named `*.tfvars` on command line or keep files names exactly as `terraform.tfvars` or `*.auto.terraform.tfvars` to load these automatically

  `terraform apply -var-file="testing.tfvars"`

- pass variables using `-var` command line option at the time of running `terraform apply` and `plan`. The `-var` option can be used any number of times in a single command

  `terraform apply -var="image_id=ami-abc123"`

- or using environment variables named `TF_VAR_` followed by the name of a declared variable

**order of variable precedence**

> environment variable << terraform.tfvars << \*.auto.tfvars << -var or -var-file options(the last value found overrides the previous values)

### Outputs

Resource instances managed by Terraform each export attributes whose values can be used elsewhere in configuration

A child module can use outputs can be used in parent module as `module.<MODULE NAME>.<OUTPUT NAME>`

When using remote state, root module outputs can be accessed by other configurations via a terraform_remote_state data source.

```
output "instance_ip_addr" {
  value = aws_instance.server.private_ip
  description =
  sensitive =
  depends_on = [

  ]
}
```

### Locals

A local value assigns a name to an expression, constant, tags, allowing it to be used multiple times within a module without repeating it.

## State, Backend and Data Block

### State

Terraform store state in `terraform.tfstate` file(local, remote),to map managed infrastructure with configuration.

> keep `-refresh=false` flag for `plan` and `apply` to improve the performance

**import** we can import resource into terraform state file, which were created from using means then terraform. This will help in migrating to terrafrom for an existing infra.

### Backend

A "backend" determines where Terraform state is stored. and how an operation such as apply is executed.

Backend are configured as part of terraform block itself

```
terraform {
  backend "consul" {
  }
}
```

### Data Block

Use of data sources allows a Terraform configuration to make use of information defined outside of current configuration.

data sources allow Terraform to access and read data from provider specific objects/resources i.e. [azure data source](https://www.terraform.io/docs/providers/azurerm/d/advisor_recommendations.html)

```
data "azurerm_subnet" "example" {
  name                 = "backend"
  virtual_network_name = "production"
  resource_group_name  = "networking"
}

output "subnet_id" {
  value = data.azurerm_subnet.example.id
}
```

## Functions

https://www.terraform.io/docs/configuration/functions.html

TF provides various functions for encoding, hash and crypto, filesystem, date time, ip calculations, collections, string and numeric functions, type conversion

> Use `terraform console` command to test functions.

`can` - special function, use for validations

## TerraTest

https://terratest.gruntwork.io/
