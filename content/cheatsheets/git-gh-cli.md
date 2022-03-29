---
title: Git & Github Cheatsheet
date: "2021-02-13T00:00:00+08:00"
cover: "cover.png"
tags: 
  - git
  - github
  - cheatsheet
keywords: 
  - git
  - github
  - cheatsheet
description: ""
showFullContent: false
readingTime: false
---

# Git

## branch
{{< carbon lang="shell" >}}
# checkout from tag 
git checkout tags/v1.0.0 -b v1.0.0
{{< /carbon >}}

## tag

{{< carbon lang="shell" >}}
# list newest 10 tags
git tag -l | tail
{{< /carbon >}}

## submodule

{{< carbon lang="shell" >}}
# checkout from tag 
git submodule add -f https://github.com/panr/hugo-theme-terminal.git themes/terminal
{{< /carbon >}}

# Github CLI


# Git Ignore

https://github.com/github/gitignore

## Frequently Used

### misc

```bash
# macOS
.DS_Store

# Vi / Vim
*.swp

```