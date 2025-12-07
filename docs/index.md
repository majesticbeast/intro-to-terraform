---
layout: default
title: Home
---

# Introduction to Terraform

## What is Terraform?

> HashiCorp Terraform is an infrastructure as code tool that lets you define both cloud and on-prem resources in
> human-readable configuration files that you can version, reuse, and share.

Or, put simply, its a tool that allows you to write configuration code (HCL) to create infrastructure.

However, it's much more than that, because in reality, Terraform is just a wrapper around any API. This can be an API
on your local system, Docker engine, GitHub, the Domino's Pizza ordering API, AWS/GCP/Azure. Literally, anything that
has an API, can in theory have a Terraform wrapper.


---

## Why use Terraform?

The better question is, "why use Infrastructure as Code?". I won't get into too much of why you should (or shouldn't)
use Terraform over other IaC tools.

There are tons of benefits of IaC, just a couple though:
- Versioned control of your infrastructure (making for ~~easy~~ easier rollbacks)
- Foolproof reproducibility across environments (simply merge lower environment branch into a higher one)
- Self-documenting infrastructure (no clicking through 300 services and multiple regions in the AWS console to see what
    you have running)


---

## How do I write Terraform?

Well, you actually write HashiCorp Configuration Language (HCL). HCL is a configuration language used across all
HashiCorp's products like Terraform, Vault, Nomad, and Packer. Terraform is the binary that you can run to turn your
HCL into AWS infrastructure, GCP infrastructure, local system files, GitHub repos, or a ton of other things.

Let's take a quick look at what HCL looks like:
```terraform
resource "aws_s3_bucket" "my_bucket" {
    bucket = "my-tf-test-bucket"

    tags = {
        Name        = "my bucket"
        Environment = "dev"
    }
}
```
There you have it, when processed by Terraform that code will create an AWS S3 Bucket.

---

## Prerequisites

- Terraform installed
- Basic command line knowledge
- An account with <INSERT REMOTE API PROVIDER HERE>
- Text editor

---

**Next:** [Your First Resource](first-resource.md)
