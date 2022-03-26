---
title: AWS CLI Cheatsheet
date: "2021-01-23T00:00:00+08:00"
cover: "images/cover-aws-cli-cheatsheet.png"
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
# get current identity
aws sts get-caller-identity
```

```bash
# assume role and set credentials to env variables, with jq
ROLE_ARN=arn:aws:iam::000000000000:role/your-role
eval $(aws sts assume-role \
        --role-arn ${ROLE_ARN} \
        --role-session-name sess-name \
        | jq -r '.Credentials | "export AWS_ACCESS_KEY_ID=\(.AccessKeyId) AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey) AWS_SESSION_TOKEN=\(.SessionToken)"'
      )
```

```bash
# assume role and set credentials to env variables, with sed & awk
ROLE_ARN=arn:aws:iam::000000000000:role/your-role
eval $(aws sts assume-role \
        --role-arn ${ROLE_ARN} \
        --role-session-name sess-name \ 
        | egrep '(SecretAccessKey|SessionToken|AccessKeyId)' \
        | awk -F'"' '{print "export AWS"toupper(gensub(/([A-Z])/, "_\\1", "g",$2))"="$4}'
      )
```

## aws ec2
```bash
# login to ec2 shell via session manager
aws ssm start-session --target i-aaaaaaaaaaaaaaaaaa
```

```bash
# port-forward
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