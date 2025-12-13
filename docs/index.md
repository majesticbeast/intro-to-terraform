---
layout: default
title: Home
---

# Introduction to Terraform

## What is Terraform?

> HashiCorp Terraform is an infrastructure as code tool that lets you define both cloud and on-prem resources in
> human-readable configuration files that you can version, reuse, and share.

Or, put simply, it's a tool that allows you to write configuration code (HCL) to create infrastructure.

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

One HUGE benefit that comes as a logical consequence of item 3 above, is that when you're done testing something, you can
delete all your infrastructure with a single command.

---

## How do I write Terraform?

You write in the Terraform Language. This is a domain-specific language that uses the HashiCorp Configuration Language (HCL)
toolkit for its syntax.

While HCL is the low-level syntax used across many HashiCorp tools (defining how to write blocks, arguments, and expressions),
the Terraform Language extends this with specific constructs for infrastructure—such as Resources, Data Sources, and Providers.
Terraform is also the binary that parses this language to orchestrate APIs for AWS, Azure, GitHub, and more.

Let's take a quick look at what Terraform looks like:
```terraform
resource "aws_s3_bucket" "my_bucket" {
    bucket = "my-tf-test-bucket"

    tags = {
        Name        = "my bucket"
        Environment = "dev"
    }
}
```
There you have it, that code will create an AWS S3 Bucket.

---

## Prerequisites

- Terraform installed
- Basic command line knowledge
- An account with <INSERT REMOTE API PROVIDER HERE>
- Text editor

---

### Up next

We create our first resource using Terraform

-> [Your First Resource](first-resource.html)
