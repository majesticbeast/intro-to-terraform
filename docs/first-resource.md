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
and then whenever those dependencies change (like when changing "providers" or module calls).

In the plan phase, Terraform compares your desired configuration against the current state and shows you exactly what
changes it will make (create, update, or destroy resources) without actually making them.

In the apply phase, Terraform executes the planned changes by making the actual API calls to create, modify, or delete
your infrastructure resources.

Each phase is run by simply adding the phase as a subcommand to your `terraform` command, e.g. `terraform init`.

## Running Terraform

We'll go deeper into what all is happening later on, but again, I just want to get you familiar with the Terraform
workflow first.

### Init phase
Open your terminal, and navigate to your working directory (where your `my_file.tf` file is). Run `terraform init`, and
you should have output similar to this:
```bash
$ terraform init
Initializing the backend...
Initializing provider plugins...
- Finding latest version of hashicorp/local...
- Installing hashicorp/local v2.6.1...
- Installed hashicorp/local v2.6.1 (signed by HashiCorp)
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

```

We can see this causes some stuff to get downloaded, it creates a lock file `.terraform.lock.hcl`, we will go into
these a bit later.

### Plan phase
In the same working directory, execute your Terraform plan:
```bash
$ terraform plan
local_file.my_file: Refreshing state... [id=68e1777a4b29d26167361a925bfdf281e2baf387]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # local_file.my_file will be created
  + resource "local_file" "my_file" {
      + content              = "Dope file contents"
      + content_base64sha256 = (known after apply)
      + content_base64sha512 = (known after apply)
      + content_md5          = (known after apply)
      + content_sha1         = (known after apply)
      + content_sha256       = (known after apply)
      + content_sha512       = (known after apply)
      + directory_permission = "0777"
      + file_permission      = "0777"
      + filename             = "./hello.txt"
      + id                   = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
```

The output shown lets you know the results of what will happen if you run `terraform apply`. I want to direct your
attention to penultimate line with text on it. We can see that one resource will be created, zero changed, and zero
destroyed. This is in line with what we expect from this change, so lets go ahead and run the apply.

### Apply phase

In the same working directory, execute your Terraform apply. I'm not going to paste the initial output of the apply
command here, as it should be identical to your `terraform plan` output, with one exception. It will ask you if you
want to perform the apply. Anything other than "yes" will result in the apply not running. So go ahead and type yes and
hit enter.

```bash
local_file.my_file: Creating...
local_file.my_file: Creation complete after 0s [id=68e1777a4b29d26167361a925bfdf281e2baf387]
```

This should essentially be an instant creation of the file we specified. If you run `ls`, you should see your new file
on disk.

When we start working with remote resources you'll find that plans and applies can take MUCH longer, depending on the
provider, the resource, and how many resources your `.terraform.tfstate` file has in it. Creating an AWS RDS instance for
instance can take well over 20 minutes for that resource alone, because in that case you're waiting on the instance to
become ready.

### Oh there's one more phase, kind of...

The last phase I didn't mention above is not really a distinct phase, as it is still really just a plan and apply. But
its the subcommand `destroy`. This will do a destroy run and delete, remove, or otherwise destroy everything you
created with your `apply`. Let's delete our file. Go ahead and run `terraform destroy`. You'll see it'll be the same
plan and apply workflow, except the summary line stating what will be added, changed, or destroyed will look like:


```bash
Plan: 0 to add, 0 to change, 1 to destroy
```

If that's what you see, type yes and hit enter. If you run `ls` again, you'll see the file is gone.


## Terraform workflow diagram

It's a simple loop, no different from programming languages where you modify code, run tests, and commit/merge.

```
Write/Update files -> Plan -> Apply -\
     ^-------------------------------/
```

## Summary

In this lesson we got a high-level look at the typical Terraform workflow. We created our first `.tf` file, we
initialized our project, ran a plan to see what changes would happen if we did an apply, and then we did the apply
to create a file, and then we deleted the file with a destroy run.

---

**Next:** [Your First Resource](providers-lockfile-statefile.html)

