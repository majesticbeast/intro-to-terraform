---
layout: default
title: Home
---

# Project Setup

With this lesson, we begin our actual project we will work on for the rest of the course. While this is a Terraform
course, we will be using AWS and a few of their core services. AWS knowledge is not required, but familiarity with AWS
or cloud offerings will be helpful. A lot of "being good at Terraform" really means "being good at <insert cloud
provider>". Terraform itself is actually quite simple.

## General Terraform setup

It's important to note that Terraform treats all `.tf` files in the same directory as one giant file. Much like Go. Also
like go, if you put `.tf` files in a subfolder, that subfolder becomes its own "module". We won't be using modules right
now, but just keep that in mind. 

## File structure

From here on out we will be using the [style guide](https://developer.hashicorp.com/terraform/language/style) to ensure
we conform to best practices and standards when naming our files and folders.

Create your project with the following structure:
```
workspace_root:
    - main.tf
    - variables.tf
    - outputs.tf
    - terraform.auto.tfvars
```

### File breakdown

**main.tf* contains all your resource and data source blocks (we'll explore data source blocks later).  
**variables.tf** contains your variable declarations.
**outputs.tf** contains outputs, or values that your code returns to its caller.
**terraform.auto.tfvars** contains your variable definitions.

So you use your variables in `main.tf`, you declare your variables in `variables.tf`, then you give those variables
values in your `.tfvars` files. Then whatever you want to output (like an S3 URL or role ARN) you add to `outputs.tf`.
Think of `outputs.tf` as your return values from a function.

Remember, all your `.tf` files in the same folder are treated as the same file by Terraform. We could just as easily use
a single file and throw everything in there.

## The project

It may feel dated, but we'll be building a standard 3-tier web app that uses virtual machines and not containers.
Well... we'll be building the infrastructure to host it. There will be little to no application code. For those not
familiar, a 3-tier web app consists of a frontend (i.e. the website), the backend (the API), and a database.

This is not your typical PaaS deployment. We are responsible for the underlying architecture all the way down to the
network layer.

### 