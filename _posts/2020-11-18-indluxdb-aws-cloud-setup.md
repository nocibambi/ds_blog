---
title: Setting up AWS Cloud for InfluxDB
description: How to set up AWS Cloud for InfluxDB?
categories: [InfluxDB, AWS]
toc: false
layout: post
---
Here are the preparation steps you need to do if you would like to use AWS Cloudwatcher with InfluxDB.

Based on this [post](https://www.influxdata.com/blog/running-influxdb-on-aws-with-cloud-formation/)

1. [Create an AWS account](https://aws.amazon.com/free/)
2. Select the appropriate region:
    ![aws region select](/images/influxdb/aws_cloudformation/influxdb-aws-region.png)
3. Search for "EC2" in the **Find Services** search box and select **EC2 (Virtual Servers in the Cloud)** (Please not that you have to provide credit card information and it may take up to 24 hours for amazon to activate):
    ![aws ec2 select](/images/influxdb/aws_cloudformation/influxdb-aws-ec2-select.png)
5. Select **Key Pairs** and the create a key pair: ![key-pairs](/images/influxdb/aws_cloudformation/influxdb-aws-ec2-key-pairs-menu.png)
6. Download the keys.
7. Make the key readable only by the owner: `chmod 400 name-of-the-key.pem`
8. Open **IAM** from the **Services menu**: ![open-aim](/images/influxdb/aws_cloudformation/screenshot-eu-central-1.console.aws.amazon.com-2020.11.19-06_05_58.png)
9. Create a user and grant permissions to it:
    1. Select **Add user**: ![iam-new-user](/images/influxdb/aws_cloudformation/screenshot-console.aws.amazon.com-2020.11.19-06_11_48.png)
    2. Name the user and add access rights: ![iam-new-user-access](/images/influxdb/aws_cloudformation/influxdb-aws-iam-new-user-access.png)
    3. On the **Set Permissions** screen click on **Add user to group** and **Create a group**: ![iam-new-user-new-group](/images/influxdb/aws_cloudformation/influxdb-aws-iam-new-user-create-group.png)
    4. Define a name and add a policy for the new group: ![iam-new-group](/images/influxdb/aws_cloudformation/influxdb-aws-iam-group-permissions.png)
    5. At the end of the process you will see your access details: ![iam-new-user-success](/images/influxdb/aws_cloudformation/influxdb-aws-iam-new-user-success.png)
    
    Note that AWS does not save your secret after you close your final screen. On the other hand, you will always be able to create a new key for your users.
10. Install the AWS CLI on your system.

    On a linux machine, you can do this as here:
    ```shell
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install
    ```

    Verify your version:
    ```shell
    aws --version
    ```

    For further instructions [see here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

11. Also create a folder where you will store your keys and your yaml configuration.

    ```shell
    mkdir ~/Projects/influxdb_aws_cloudformation
    cd ~/Projects/influxdb_aws_cloudformation
    mv ~/Downloads/name-of-the-key.pem .
    touch config.yml
    ```

12. Configure AWS with `aws configure`. This is where you need to add your **Access Key ID**, your **Secret Access Key**, and the **region name** (e.g. `eu-central-1`). You can skip the output format.
