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

## acme.sh  create certificate with letsencrypt
https://github.com/acmesh-official/acme.sh

{{< carbon lang="shell" >}}
# for aliyun dns
export Ali_Key=
export Ali_Secret=

~/.acme.sh/acme.sh --issue -d example.com -d *.example.com  --dns dns_ali

# for godaddy
export GD_Secret=
~/.acme.sh/acme.sh --issue -d example.com -d *.example.com  --dns dns_gd
{{< /carbon >}}
