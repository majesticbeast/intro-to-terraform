---
layout: default
title: Home
---

# Your First Resource

Lets dive immediately into a piece of code that you can run now, without any accounts, and get a feel for the entire
Terraform workflow.

Open your text editor and create a file `my_file.tf`, and add the following code:

```tf
resource "local_file" "my_file" {
    name    = "${path.module}/hello.txt"
    content = "Dope file contents"
}
```

That's it! Go ahead and open your terminal, make sure your working directory is the same directory the `my_file.tf` is in.


