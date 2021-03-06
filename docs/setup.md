---
id: setup
title: Prerequisites
---

## AWS

The installation assumes that the following AWS resources have been created/set-up and can be referenced in the us-east-1 region:

- AWS account
- [AWS CLI](https://aws.amazon.com/cli/)
- IAM credentials
- S3 bucket
- VPC
  - Subnets
  - SG
- Trusted Advisor

Review [Trusted Advisor](https://aws.amazon.com/premiumsupport/ta-iam/) for more customization.

## Permission

### Credentials

You should have valid credentials to interact with the AWS CLI. This tool will allow you to use the `--profile` argument.

### Cloudformation

Automated Cloud Advisor is provided as a set of [Cloudformation](https://aws.amazon.com/cloudformation/) templates, the deployment of which leading to the provisioning of the various required resources.

The deployment of the templates requires the caller to have enough [IAM](https://aws.amazon.com/iam/) access rights for the creation of the following resources:

```yml
3: 'AWS::IAM::ManagedPolicy'
3: 'AWS::IAM::Role'
3: 'AWS::Lambda::Function'
2: 'AWS::Events::Rule'
2: 'AWS::Lambda::Permission'
1: 'AWS::Lambda::EventSourceMapping'
1: 'AWS::DynamoDB::Table'
1: 'AWS::Elasticsearch::Domain'
```

## Security

### S3

Your S3 bucket will be used as a reference for the lambda source code and should have the proper version enabled/ACL/Bucket Policy attached.

### VPC

As a prerequisite, the deployment of the Kibana dashboard requires the existence [VPC](https://aws.amazon.com/vpc/) with subnets and security groups.

## Cost

Infrastructure configurations can be overwritten to tweak costs. Average estimation provided below will vary on your resources.

[Lambda](https://aws.amazon.com/lambda/pricing/) - Less than $1 per month

|Average    |Refresh|Index|Stream|
|-----------|-------|-----|-----|
|Memory     |128mb  |128mb|128mb|
|Execution  |1000ms |500ms|50ms |
|Concurrency|1      |100  |4    |

[DynamoDB](https://aws.amazon.com/dynamodb/pricing/) - Less than $1 per month.

- Read/Write - BillingMode is set to PAY_PER_REQUEST

|Average|Read|Write|
|-------|----|-----|
|Consume|0   |4    |

|Average|Stream   |
|-------|---------|
|Batch|1,100 bytes|

[ElasticSearch](https://aws.amazon.com/elasticsearch-service/pricing/?nc=sn&loc=3) - Less than $1,000 per month.

|Custer  |Master                |Nodes                 |
|--------|----------------------|----------------------|
|Type    |r5.large.elasticsearch|r5.large.elasticsearch|
|Instance|3                     |2                     |

|       |EBS|
|-------|---|
|type   |gp2|
|IOPS   |0  |
|GB     |20 |

## Installation

### Quick Setup

You can either run this script or manually set up the infrastructure in the next few steps. The script assumes that [jq](https://stedolan.github.io/jq/) (a json parsing library) is installed on your system and that you have valid credentials that can be used to create AWS resources through CloudFormation.

```bash
# Download installer with npm -g
npm i -g automated-cloud-advisor
# Execute Binary
automated-cloud-advisor
```

You will be prompted theses questions:

![alt-text](/automated-cloud-advisor/img/installation.png)

Now you can build the Kibana dashboard by clicking [here](https://disneystreaming.github.io/automated-cloud-advisor/docs/view).

### Manual Setup

```bash
# Clone the repo
git clone git@github.com:disneystreaming/automated-cloud-advisor.git
# Change directory to the repo
cd automated-cloud-advisor
```

Go to the next page ->
