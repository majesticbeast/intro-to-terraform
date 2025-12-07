---
layout: default
title: Home
---

# Introduction to Terraform

## What is Terraform?

> HashiCorp Terraform is an infrastructure as code tool that lets you define both cloud and on-prem resources in
> human-readable configuration files that you can version, reuse, and share.

Or, put simply, its a tool that allows you to write configuration code (HCL) to create infrastructure.

However, it's much more than that, because in reality, Terraform is just a wrapper around any API. This can be your
local system, Docker, GitHub, Domino's Pizza, AWS/GCP/Azure. Literally, anything that has an API, can in theory have a
Terraform wrapper.

---

## How do I write Terraform?

Well, you actually write HashiCorp Configuration Language (HCL). HCL is a configuration language used across all
HashiCorp's products like Terraform, Vault, Nomad, and Packer. Terraform is the binary that you can run to turn your
HCL into AWS infrastructure, GCP infrastructure, local system files, GitHub Repos, or a ton of other things.

Let's take a quick look at what HCL looks like:
```tf
resource "aws_s3_bucket" "my_bucket" {
    bucket = "my-tf-test-bucket"

    tags = {
        Name        = "my bucket"
        Environment = "dev"
    }
}

```
There you have it, that code when processed by Terraform will create an AWS S3 Bucket.

---

## What next?

asdf fdas
