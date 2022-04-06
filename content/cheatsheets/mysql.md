---
title: MySQL Cheatsheet
date: "2021-02-15T00:00:00+08:00"
cover: "cover.png"
tags: 
  - mysql
  - cheatsheet
keywords: 
  - mysql
  - cheatsheet
description: ""
---

{{< carbon lang="shell" >}}
# create user and grant access
create user 'testuser'@'%';
GRANT ALL PRIVILEGES ON testdb.* To 'testuser'@'%' IDENTIFIED BY 'password';
{{< /carbon >}}
