---
title: "k8s service troubleshoot"
date: "2021-03-23T21:00:00+08:00"
tags: 
- k8s
- troubleshoot
keywords:
- k8s
- troubleshoot
---

## 运行在kubernetes 的服务排错思路

1. 

{{< carbon lang="shell" >}}
k -n app describe po POD_NAME | grep -C 5 'Last State'
      ./bridge-api
      --config
      config.toml
    State:          Running
      Started:      Fri, 22 Apr 2022 22:04:13 +0800
    Last State:     Terminated
      Reason:       Error
      Exit Code:    2
      Started:      Fri, 22 Apr 2022 21:54:45 +0800
      Finished:     Fri, 22 Apr 2022 22:01:29 +0800
    Ready:          True
{{< /carbon >}}

https://containersolutions.github.io/runbooks/posts/kubernetes/crashloopbackoff