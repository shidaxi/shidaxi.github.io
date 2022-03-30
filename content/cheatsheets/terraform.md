---
title: Terraform Cheatsheet
date: "2022-03-12T00:00:00+08:00"
cover: "cover.png"
tags: 
  - terraform
  - terragrunt
  - cheatsheet
keywords: 
  - terraform
  - terragrunt
  - cheatsheet
---

# Useful Snippets

{{< carbon lang="javascript" >}}

// work around for error "Inconsistent conditional result types" when use tenary operator between different maps
locals {
  map_bool = {
    true = local.your_map
    false = {}
  }
}
module "s3-bucket" {
    map_key = local.map_bool[var.condition_var]
}
{{< /carbon >}}