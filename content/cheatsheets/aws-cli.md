---
title: Kubernetes Cheatsheet
date: "2021-01-23T00:00:00+08:00"
cover: "images/cover-kubernetes-resources.png"
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

## aws configure
```bash
# aws cli related environement variables
export AWS_PROFILE=
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_SESSION_TOKEN=
export AWS_REGION=ap-southeast-1

export AWS_DEFAULT_OUTPUT=json
export AWS_DEFAULT_REGION=ap-southeast-1
```

## aws sts
```bash

```

## aws ec2
```bash

```

## aws s3
```bash
# list bucket
aws s3 ls
```

```bash
# rename folder
aws s3 --recursive mv s3://your-bucket/path/to/foo s3://your-bucket/path/to/bar
```

## aws eks

```bash
# check web identity
env | grep AWS_
```