---
title: Setting up AWS Cloud for InlfuxDB
description: How to set up AWS Cloud for InfluxDB?
categories: [InfluxDB, AWS]
toc: false
layout: post
---
Here are the preparation steps you need to do if you would like to use AWS Cloudwatcher with InfluxDB.

Based on this [post](https://www.influxdata.com/blog/running-influxdb-on-aws-with-cloud-formation/)

1. [Create an AWS account](https://aws.amazon.com/free/)
2. Select the appropriate region

![aws region select](/images/influxdb/influxdb-aws-region.png)

3. Create a new EC2 instance (Please not that it may take up to 24 hours for amazon to activate).

![aws ec2 select](/images/influxdb/influxdb-aws-ec2-select.png)

1. Select **Key Pairs** and the create a key pair
2. Download keys
3. Make the key readable only by the owner: `chmod 400 influx_private.pem`
4. Open **IAM** in **Security** section of the **Services menu**.
5. Create a user and grant permissions to it.
6. Generate and access key ID and a secred for the AWS command line interface.
