---
layout: default
title: Home
---

# Your First Resource

## Creating our file

Lets dive immediately into a piece of code that you can run now, without any accounts, and get a feel for the entire
Terraform workflow.

Open your text editor and create a file `my_file.tf`, and add the following code:

```tf
resource "local_file" "my_file" {
    name    = "${path.module}/hello.txt"
    content = "Dope file contents"
}
```

That's it. It should be relatively obvious what this code will do. That's one huge advantage of using Terraform for
your IaC, it is *very* easy to see what you are creating.

Before we run Terraform and create our local file, let's go over the Terraform workflow.

## The Terraform workflow

Running Terraform consists of 2-3 phases.

- init
- plan
- apply

The init phase initializes the working directory and downloads necessary dependencies. This means you run it initially,
and then whenever those dependencies change (like when changing providers or module calls).

In the plan phase, Terraform compares your desired configuration against the current state and shows you exactly what
changes it will make (create, update, or destroy resources) without actually making them.

In the apply phase, Terraform executes the planned changes by making the actual API calls to create, modify, or delete
your infrastructure resources.

Each phase is run by simply adding the phase as a subcommand to your `terraform` command, e.g. `terraform init`

## Running Terraform


