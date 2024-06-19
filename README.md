<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.

Permission is hereby granted, free of charge, to any person obtaining a copy of this
software and associated documentation files (the "Software"), to deal in the Software
without restriction, including without limitation the rights to use, copy, modify,
merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
-->
# Contacts dashboard for Ground Station

## Table of contents:

* [1. Introduction](./README.md#1-introduction)

* [2. Key features](./README.md#2-key-features)

* [3. Reference architecture](./README.md#3-reference-architecture)

* [4. Deploying the solution](./README.md#4-deploying-the-solution)

* [5. Credits](./README.md#5-credits)


## 1. Introduction

The Contacts dashboard for Ground Station is a solution for customers to have a centralized, single pane of glass view to check the information related to their AWS Ground Station contacts for their entire satellite fleet. This information includes contact status, ground station site, satellite, mission profile, etc but also includes a mapping to their Cost and Usage Report so that customers can get the billing details of those contacts in the same place.

The dashboard is deployed in Amazon Quicksight and the solution is made available through a set of AWS CloudFormation templates that customers can deploy in their AWS environment.

![AWS GS dashboard](/static/dashboard.png)

## 2. Key features

* **Single pane of glass:** This solution creates and maintains a centralized dataset of AWS Ground Station contact details and displays this information in an accessible way using a Quicksight dashboard. This helps users manage the AWS Ground Station contacts for their satellite fleet over all AWS Ground Station sites. Contact information is mapped to corresponding details in AWS Cost and Usage Report so that billing information for each contact is also presented as part of the dashboard.

* **Ease of deployment:** The whole solution can be deployed and updated using a set of CloudFormation templates. With the exception of Quicksight initial configuration, no manual steps are required.

## 3. Reference architecture

![Reference architecture](/static/ref-architecture.png)

1. Upon solution deployment, an AWS Lambda function retrieves the list of historic AWS Ground Station contacts for the account.

2. An Amazon EventBridge rule matching AWS Ground Station contact state changes sends the events to Lambda for processing. Any processing failures are notified via email using Amazon SNS.

3. Events related to AWS Ground Station contact state changes performed in additional AWS regions are routed to the EventBridge bus in the central region.

4. AWS Ground Station contact information is stored in an Amazon Simple Storage Service (Amazon S3) bucket in the central region.

5. AWS Cost and Usage Report (CUR) publishes billing information for the account to an S3 bucket in the central region.

6. AWS Glue periodically crawls the S3 buckets containing the AWS Ground Station contacts and CUR information, automatically inferring database and table schemas and storing them in the Glue Data Catalog.

7. Amazon Athena uses table metadata from the Glue Data Catalog to execute queries over the information stored in the S3 buckets, joining AWS Ground Station contact details with corresponding billing information.

8. The resulting curated data is displayed on a Quicksight dashboard, enabling centralized data visualization, as well as easy filtering and extraction of insights from the source data.


## 4. Deploying the solution

Find a step by step walkthrough in our [Deployment guide](/DEPLOYMENT.md).

## 5. Credits

Source code required to deploy the AWS Cost and Usage report as part of this solution has been adapted from [aws-cudos-framework-deployment](https://github.com/aws-samples/aws-cudos-framework-deployment/) project.
