---
title: OpenSSL Cheatsheet
date: "2020-03-13T00:00:00+08:00"
cover: "cover.png"
tags: 
  - openssl
  - certificate
  - cheatsheet
keywords: 
  - openssl
  - certificate
  - cheatsheet
description: ""
showFullContent: false
readingTime: false
---

{{< carbon lang="shell" >}}
# check https server certificate 
openssl s_client -showcerts -servername www.example.com -connect httpbin.org:443 </dev/null
{{< /carbon >}}