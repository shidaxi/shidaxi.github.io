---
title: AWS CLI Cheatsheet
date: "2021-02-23T00:00:00+08:00"
cover: "cover.png"
tags: 
  - aws
  - awscli
  - cheatsheet
keywords: 
  - aws
  - awscli
  - cheatsheet
description: ""
showFullContent: false
readingTime: false
---

{{< carbon lang="shell" >}}
# check where 301/302 redirect to
curl https://httpbin.org/status/301 -i -s | grep -i location
{{< /carbon >}}