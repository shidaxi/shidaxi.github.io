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
{{< carbon lang="shell" height="320px" >}}
# aws cli related environement variables
export AWS_PROFILE=
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_SESSION_TOKEN=
export AWS_REGION=ap-southeast-1
export AWS_DEFAULT_OUTPUT=json
export AWS_DEFAULT_REGION=ap-southeast-1                                   
{{</ carbon >}}

## cli output filtering

For basic syntax, please refer to 
https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html

This article only share some practical examples.

### server side filtering with option --filter

Server-side filtering in the AWS CLI is provided by the AWS service API. The AWS service only returns the records in the HTTP response that match your filter, which can speed up HTTP response times for large data sets. Since server-side filtering is defined by the service API, the parameter names and functions vary between services.

**Notice:**
* --filter and --filters does not support regular expressions. 
* --filter-expression does support expression, but only a few aws service support this option

```bash

```

### client side filtering with optioin --query

The AWS CLI provides built-in JSON-based client-side filtering capabilities with the --query parameter. The --query parameter is a powerful tool you can use to customize the content and style of your output. The --query parameter takes the HTTP response that comes back from the server and filters the results before displaying them. Since the entire HTTP response is sent to the client before filtering, client-side filtering can be slower than server-side filtering for large data-sets.

```bash

```

### Client Side Filtering with jq

```bash

```

## aws sts
{{< carbon lang="shell" height="220px" >}}
# get current identity
aws sts get-caller-identity                                                          
{{< /carbon >}}

{{< carbon lang="shell" height="580px" >}}
# assume role and set credentials to env variables, with jq
ROLE_ARN=arn:aws:iam::000000000000:role/your-role
eval $(aws sts assume-role \
        --role-arn ${ROLE_ARN} \
        --role-session-name sess-name \
        | jq -r '.Credentials | "export AWS_ACCESS_KEY_ID=\(.AccessKeyId) AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey) AWS_SESSION_TOKEN=\(.SessionToken)"'
      )
# assume role and set credentials to env variables, with sed & awk
ROLE_ARN=arn:aws:iam::000000000000:role/your-role
eval $(aws sts assume-role \
        --role-arn ${ROLE_ARN} \
        --role-session-name sess-name \ 
        | egrep '(SecretAccessKey|SessionToken|AccessKeyId)' \
        | awk -F'"' '{print "export AWS"toupper(gensub(/([A-Z])/, "_\\1", "g",$2))"="$4}'
      )
{{< /carbon >}}

## aws ec2

{{< carbon lang="shell" height="470px" >}}
# find ec2 with --filters
aws ec2 describe-instances \
    --filters Name=instance-type,Values=t2.micro,t3.micro 
              Name=availability-zone,Values=us-east-2c                                        

# find ec2 with --filters by tag
aws ec2 describe-instances \
    --filters Name=tag:Owner,Values=my-team

# find ec2 with --filters by tag
aws ec2 describe-instances \
    --filters "Name=tag:Owner,Values=my-team"
{{< /carbon >}}

{{< carbon lang="shell" height="760px" >}}
# get only useful information of ec2 instances wich both --filters and --query
aws ec2 describe-instances \
    --filters "Name=tag:Owner,Values=my-team" \
    --query 'Reservations[*].Instances[*].{Instance:InstanceId, Name:Tags[?Key==`Name`]|[0].Value}'

# --output table makes outputs more human readable.
aws ec2 describe-instances \
    --filters Name=tag-key,Values=Name \
    --query 'Reservations[*].Instances[*].{Instance:InstanceId,AZ:Placement.AvailabilityZone,Name:Tags[?Key==`Name`]|[0].Value}' \
    --output table

-------------------------------------------------------------
|                     DescribeInstances                     |
+--------------+-----------------------+--------------------+
|      AZ      |       Instance        |        Name        |
+--------------+-----------------------+--------------------+
|  us-east-2b  |  i-057750d42936e468a  |  my-prod-server    |
|  us-east-2a  |  i-001efd250faaa6ffa  |  test-server-1     |
|  us-east-2a  |  i-027552a73f021f3bd  |  test-server-2     |
+--------------+-----------------------+--------------------+
{{< /carbon >}}

{{< carbon lang="shell" height="320px" >}}
# login to ec2 shell via session manager
aws ssm start-session --target i-aaaaaaaaaaaaaaaaaa
                                                                                           
# port-forward

{{< /carbon >}}

## aws s3
{{< carbon lang="shell" height="320px" >}}
# list bucket
aws s3 ls
                                                                                           
# rename folder
aws s3 --recursive mv s3://your-bucket/path/to/foo s3://your-bucket/path/to/bar
{{< /carbon >}}


## aws eks

{{< carbon lang="shell" height="320px" >}}
# check web identity of pod
env | grep AWS_

# get cluster name
aws eks list-clusters

# get kubeconfig
aws eks update-kubeconfig --name your-cluster-name
                                                                                        
{{< /carbon >}}

## cloudfront

{{< carbon lang="shell" height="320px" >}}
# list distributions with id, aliases, domian name and origin
aws cloudfront list-distributions \
  | jq  '.DistributionList.Items[] | {"Id": .Id, "Aliases": .Aliases.Items, "Domain": .DomainName, "Origin": .Origins.Items[0].DomainName}'

# create an invalidation, clear cache
aws cloudfront create-invalidation --distribution-id E3NXXXXXXXXXXX --paths "/*"
{{< /carbon >}}

## acm

{{< carbon lang="shell" >}}
# describe certificates
for c in $(aws acm list-certificates \
          --query 'CertificateSummaryList[].CertificateArn' --output text)
do aws acm describe-certificate --certificate-arn $c \
    --query 'Certificate.{CertificateArn:CertificateArn, DomainName:DomainName,SubjectAlternativeNames:SubjectAlternativeNames, Status:Status,NotAfter:NotAfter}' | jq
done
{{< /carbon >}}


## route53

{{< carbon lang="shell" >}}

# list zones
aws route53 list-hosted-zones | jq
# create zone
aws route53 create-hosted-zone --name example.com --caller-reference 2014-04-01-18:47 | jq
# get zone id by zone name
aws route53 list-hosted-zones \
  --query 'HostedZones[?Name==`example.com.`] | [0].Id' 
  --output text
# list record sets by zone name
aws route53 list-resource-record-sets \
  --hosted-zone-id $(aws route53 list-hosted-zones \
                      --query 'HostedZones[?Name==`example.com.`] | [0].Id' \
                      --output text) \
  | jq
# list CNAME record sets by zone name 
aws route53 list-resource-record-sets \
  --hosted-zone-id $(aws route53 list-hosted-zones \
                      --query 'HostedZones[?Name==`example.com.`] | [0].Id' \
                      --output text) \
  --query 'ResourceRecordSets[?Type==`CNAME`] \ 
  | jq
# create record set

{{< /carbon >}}

# ecr 
{{< carbon lang="shell" >}}
# docker login ecr
aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
{{< /carbon >}}
