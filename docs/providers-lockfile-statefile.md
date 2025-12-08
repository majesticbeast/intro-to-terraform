---
layout: default
title: Home
---

# Providers, Lock Files, and State Files.

## Providers

### What are they

You've already seen mention of providers on the prior pages, but I didn't explain what they are. If you recall,
in the intro I mentioned that Terraform is really just a wrapper over an existing API. That's *truish*. To be exact, a
provider is the actual wrapper around any given API or set of related APIs.

Let's look at a templated version of the first line of a typical block of Terraform code:
```
[BLOCK_TYPE] [RESOURCE_TYPE] [RESOURCE_NAME] {
```
**BLOCK_TYPE** defines what kind of HCL block you're declaring  
**RESOURCE_TYPE** tells Terraform which provider and which resource from that provider you're declaring  
**RESOURCE_NAME** is a logical identifier for you and Terraform to use to reference the resource in other parts of your
config. It is NOT used for the actual name of the resource you create. It never leaves your Terraform environment.

Recall this line from the last lesson:
```
resource "local_file" "my_file" {
```
`resource` is the BLOCK_TYPE, `local_file` is the RESOURCE_TYPE, and `my_file` is the RESOURCE_NAME.

If you haven't guessed it by now, the first part of the RESOURCE_TYPE is the provider name. This means that we used the
`local` provider. The `local` provider wraps the Go standard library functions for interacting with the local
filesystem. You can view the `local_file` source code [here](https://github.com/hashicorp/terraform-provider-local/blob/main/internal/provider/resource_local_file.go).
You can see the calls to functions like `os.Stat`, `os.Mkdir`, `os.WriteFile`, and others.

There's one caveat I haven't addressed yet. `local` is not the full name of the provider. Much like a GitHub repo, the
provider consists of the provider's namespace, followed by the provider type (i.e. its name). Also like GitHub repos,
there can be multiple providers all called `local` that exist in different namespaces.

This raises an important question. How do we make sure we use the correct `local` provider?

### Provider requirements
Let me introduce you to a new block of code we should be sure to include in our code:
```
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "2.6.1"
    }
  }
}
```
This prevents Terraform from having to guess which actual provider you intended to use. We specify both the namespace
and the name of the provider, along with the version to give a complete picture of which exact provider we intend to
use. You may list multiple providers in this `required_providers` block if you need to. It's probably uncommon to only
use a single provider.

In the above block of code, we set the provider's "local name" to "aws". This comes into play in the next section where
we will start configuring provider's and creating aliases.

> Note: While you can use a different "local name" for your provider, it's idiomatic to name it the same as the
> provider's preferred local name. The preferred local name is simply the prefix it uses for all it's resource types and
> as far as I have experienced, is the same as the provider's name/type.

### Provider configurations

We can (and often must) configure our providers. We didn't need to with our `local_file` example, but with anything that
calls a remote API, you'll likely need to do some configuration. This can involve passing in username, tokens, setting
regions, etc. Let's take a look at how we do that:

```
provider "[PROVIDER_NAME]" {
    [PROVIDER_ARGUMENTS]
    alias = "[ALIAS_NAME]"
}
```

**PROVIDER_ARGUMENTS** are unique to each provider.

Taking the `aws` provider as an example, we have an enormous amount of arguments we could pass it into. Some are:
- access_key
- secret_key
- token
- region
- profile
- tons more...

It's no coincidence that the available arguments will often match up to the same arguments you would pass into an AWS
client if you're using the AWS SDK. Remember, a provider is a wrapper over existing APIs. In fact, the AWS provider is
a wrapper around the AWS SDK for Go. See the provider source code for creating S3 buckets [here](https://github.com/hashicorp/terraform-provider-aws/blob/main/internal/service/s3/bucket.go)
and note the imports at the top of the file, along with the actual SDK function calls like `input := &s3.CreateBucketInput`.

The "alias" argument is slightly more difficult to understand. Sometimes you'll want to use the same provider but with
different configurations. Think of this as just creating multiple SDK clients with different configurations. Maybe you
want to create 2 S3 buckets, one in us-west-2 and one in us-east-1, so you can back up the one bucket with the other.

```
provider "aws" {
    region = "us-west-2"
}

provider "aws" {
    alias  = "east"
    region = "us-east-1"
}

resource "aws_s3_bucket" "primary" {
  bucket = "my-primary-data-store"
}

resource "aws_s3_bucket" "backup" {
  provider = aws.east
  bucket   = "my-backup-data-store"
}
```

When you have a resource and you don't provide the `provider` argument, it uses the default provider that you have
configured. If you don't configure the provider at all, it will use default settings (credentials, region, etc).

## Lock files

The lock file `.terraform.lock.hcl` records the exact versions of providers that Terraform downloaded during the init
phase. It's essentially Terraform's version of the `package-lock.json` or `go.mod`. You will have one in your directory
by now, feel free to go check it out.

## State files

The `terraform.tfstate` state file is the most important piece of your workspace. It is Terraform's database that tracks
the current state of your real-world infrastructure (or whatever it is you used Terraform for, like creating a local
file). Since we don't have any real resources yet, we won't get into it too much. For now, just know that it maps your
Terraform code to actual resources and stores metadata about those resources, as well as relationships between your
resources.

## Terraform documentation

Wondering yet where to find what arguments you can pass your provider? Or what resources you can create? And what
arguments are available to those resources? You'll want to head on over to the Terraform provider documentation [here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs).

Due to the way the provider docs are laid out, the search is horrible. So when I'm looking anything in there, I usually
just use a search engine with a few keywords, like so: `tf aws s3 bucket`.


## Summary

We went over what providers are, how to configure them, and the importance of the `required_providers` block. We also
briefly covered what the lock and state files are.

You are ready to use Terraform to go deploy resources. There's a ton of stuff yet to be covered, but if you're itching
to go deploy cloud resources, go ahead! Just remember to `terraform destroy` when done experimenting.