---
title: MySQL Cheatsheet
date: "2021-02-15T00:00:00+08:00"
cover: "cover.png"
tags: 
  - nginx
  - cheatsheet
keywords: 
  - nginx
  - cheatsheet
description: ""
---

## nginx sni proxy
{{< carbon lang="nginx" >}}
events {
  worker_connections  1024;
}
worker_processes auto;
error_log /dev/stdout notice;
pid /var/run/nginx.pid;
http {
  server {
    listen 80 default_server;
    server_name _;
    access_log off;
    return 301 https://$host$request_uri;
  }
}
stream {
  map $ssl_preread_server_name $backend {
    ~^.*\.example.com$ 127.0.0.1:443;
  }
  server {
    listen 443;
    ssl_preread on;
    proxy_connect_timeout 1s;
    proxy_timeout 3s;
    resolver 172.20.0.10;
    proxy_pass $backend;
  }
}
{{< /carbon >}}

## force https redirection

{{< carbon lang="nginx" >}}
  server {
    listen 80 default_server;
    server_name _;
    access_log off;
    return 301 https://$host$request_uri;
  }
{{< /carbon >}}