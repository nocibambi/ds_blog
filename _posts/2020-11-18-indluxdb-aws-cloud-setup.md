---
title: Setting up AWS Cloud for InfluxDB
description: How to set up AWS Cloud for InfluxDB?
categories: [AWS, EC2, InfluxDB]
toc: false
layout: post
---
Here are the preparation steps you need to do if you would like to use AWS Cloudwatcher with InfluxDB.

Based on this [post](https://www.influxdata.com/blog/running-influxdb-on-aws-with-cloud-formation/)

## Setting up an EC2 virtual server

1. [Create an AWS account](https://aws.amazon.com/free/)
1. Select the appropriate region:
    ![aws region select](/images/influxdb/aws_cloudformation/influxdb-aws-region.png)
1. Search for "EC2" in the **Find Services** search box and select **EC2 (Virtual Servers in the Cloud)** (Please not that you have to provide credit card information and it may take up to 24 hours for amazon to activate):
    ![aws ec2 select](/images/influxdb/aws_cloudformation/influxdb-aws-ec2-select.png)
1. Select **Key Pairs** and the create a key pair: ![key-pairs](/images/influxdb/aws_cloudformation/influxdb-aws-ec2-key-pairs-menu.png)
1. Download the keys.
1. Make the key readable only by the owner: `chmod 400 name-of-the-key.pem`
1. Open **IAM** from the **Services menu**: ![open-aim](/images/influxdb/aws_cloudformation/infludb-aws-select-iam.png)
1. Create a user and grant permissions to it:
    1. Select **Add user**: ![iam-new-user](/images/influxdb/aws_cloudformation/infludb-aws-iam-create-user.png)
    2. Name the user and add access rights: ![iam-new-user-access](/images/influxdb/aws_cloudformation/influxdb-aws-iam-new-user-access.png)
    3. On the **Set Permissions** screen click on **Add user to group** and **Create a group**: ![iam-new-user-new-group](/images/influxdb/aws_cloudformation/influxdb-aws-iam-new-user-create-group.png)
    4. Define a name and add a policy for the new group: ![iam-new-group](/images/influxdb/aws_cloudformation/influxdb-aws-iam-group-permissions.png)
    5. At the end of the process you will see your access details: ![iam-new-user-success](/images/influxdb/aws_cloudformation/influxdb-aws-iam-new-user-success.png)

    Note that AWS does not save your secret after you close your final screen. On the other hand, you will always be able to create a new key for your users.
1. Install the AWS CLI on your system.

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

1. Also create a folder where you will store your keys and your yaml configuration.

    ```shell
    mkdir ~/Projects/influxdb_aws_cloudformation
    cd ~/Projects/influxdb_aws_cloudformation
    mv ~/Downloads/name-of-the-key.pem .
    touch config.yml
    ```

1. Configure AWS with `aws configure`. This is where you need to add your **Access Key ID**, your **Secret Access Key**, and the **region name** (e.g. `eu-central-1`). You can skip the output format.

## Amazon Machine Image

Open the [Amazon AWS Marketplace](https://aws.amazon.com/marketplace/).

Find a public image

![Find public image](/images/influxdb/2020-11-20-influxdb-aws-find-public-image.png)

## Defining resources

Resources
Logical ID
Instance properties
AMI (Amazon Machine Image) ID

```yaml
Resources:
  Appnode:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.nano
      ImageId: ami-06a719e5f8e22c33b # The AMI instance ID
      Keyname: InfluxDB_AWS_example # The name of your key pair
      SecurityGroups:
        - !Ref AppnodeSecurityGroup # Reference the security group defined below
```

You need to define a security group. Security groups act as virtual firewalls for your incoming/outgoing traffic with the help of rules. You can read more about them [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html). Here, we define an [inbound security group](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group-ingress.html). For particular rules, [see here](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#SecurityGroupRules).

We define the security group and link it to our app node definition with the [`Ref` function](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html).

```yaml
# Define the security group
AppnodeSecurityGroup:
  Type: AWS::EC2::SecurityGroup
  Properties:
    GroupDescription: SSH enabled app nodes
    # Inbound security rule
    # Expose the HTTP port 80 with tcp to inbound traffic from andy Ipv4 addresses
    SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: '80'
        ToPort: '80'
        CidrIp: 0.0.0.0/0
```

Define Bash script to install docker and influxdb on the image.

The full yaml:

```yaml
Resources:
  AppNode:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-03d85bfa79ad10274
      KeyName: InfluxDB_AWS_example
      SecurityGroups:
        - !Ref AppNodeSG
      UserData: !Base64 |
          #!/bin/bash
          apt-get update -qq
          apt-get install -y apt-transport-https ca-certificates
          apt-key adv apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
          apt-get update -qq apt-get purge lxc-docker || true
          curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
          add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
          apt-get -y install linux-image-extra-$(uname -r) linux-image-extra-virtual
          apt-get -y install docker-ce
          usermod -aG docker unbuntu
          docker image pull quay.io/influxdb/influxdb:2.0.0-alpha
          docker container run -p 80:9999 quay.io/influxdb/influxdb:2.0.0-alpha
          wget https://dl.influxdata.com/telegraf/releases/telegraf_1.10.4-1_amd64.deb
          sudo dpkg -i telegraf_1.10.4-1_amd64.deb
  AppNodeSG:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: for the app nodes that allow ssh
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: '80'
        ToPort: '80'
        CidrIp: 0.0.0.0/0
      - IpProtocol: tcp
        FromPort: '22'
        ToPort: '22'
        CidrIp: 0.0.0.0/0
```

Create stack

```shell
aws cloudformation create-stack \
  --stack-name influxdb-trial-stack \
  --region eu-central-1 \
  --template-body file://$PWD/stack.yaml
```

![Get AMI instance DNS](/images/influxdb/2020-11-23-influxdb-aws-get-instance-dns.png)

Create a connection to our instance:

```bash
ssh -v -i InfluxDB_AWS_example.pem \
  ubuntu@ec2-3-122-XXX-XX.eu-central-1.compute.amazonaws.com
```

### Install InfluxDB
Maybe this: https://websiteforstudents.com/how-to-install-influxdb-on-ubuntu-18-04-16-04/


```bash
wget https://dl.influxdata.com/influxdb/releases/influxdb_2.0.2_amd64.deb
sudo dpkg -i influxdb_2.0.2_amd64.deb
```

Setup InfluxDB configuration settings

```bash
influx setup
```

Here you need to add a username, a password, and name your organization and your primary bucket. You can skip on the retention period question.

### Try out InfluxDB

```bash
influx bucket list
```

```bash
$ date +%s
1606723341
```

```bash
$ influx write -b test_bucket -o nocibambi@gmail.com -p s 'test_measurement,host=testHost testField="testFieldValue" 1606723341'
```
```bash
$ influx query
from(bucket: "test_bucket") |> range(start:-1h)
Result: _result
Table: keys: [_start, _stop, _field, _measurement, host]
                   _start:time                      _stop:time           _field:string     _measurement:string             host:string                      _time:time           _value:string  
------------------------------  ------------------------------  ----------------------  ----------------------  ----------------------  ------------------------------  ----------------------  
2020-11-30T07:03:07.010273692Z  2020-11-30T08:03:07.010273692Z               testField        test_measurement                testHost  2020-11-30T08:02:21.000000000Z          testFieldValue  
```


```bash
influx query
```

```flux
from(bucket: "test_bucket") |> range(start: -7d)
```


### Run influxdb service

Start the influxdb service.

```bash
sudo systemctl start influxdb.service
```

<https://devconnected.com/how-to-install-influxdb-on-ubuntu-debian-in-2019/#II_Installing_InfluxDB_20>

### Install telegraf

We need to install telegraf onto our instance.

On an Ubuntu instance this will look like this:

Adding the repository:

```bash
wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -
source /etc/lsb-release
echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" \
  | sudo tee /etc/apt/sources.list.d/influxdb.list
```


Installing telegraf:

```bash
sudo apt-get update && sudo apt-get install telegraf
```
j

```bash
telegraf --test
```

Create an example data collection setting config and test it
```bash
telegraf -sample-config --input-filter cpu:mem --output-filter influxdb > telegraf_test.conf
telegraf --config telegraf.conf --test
```



<https://docs.influxdata.com/telegraf/v1.16/guides/using_http/>

For other systems and futher information see [this link](https://docs.influxdata.com/telegraf/v1.16/introduction/installation/).

## Install Chronograph

```bash
wget https://dl.influxdata.com/chronograf/releases/chronograf_1.8.8_amd64.deb
sudo dpkg -i chronograf_1.8.8_amd64.deb
```

### Configure Telegraf

- [ ] <https://docs.influxdata.com/telegraf/v1.16/guides/using_http/>

On Ubuntu instance the config file path is at `/etc/telegraf/telegraf.conf`.

- <https://docs.influxdata.com/telegraf/v1.15/administration/configuration/>
- <https://docs.influxdata.com/telegraf/v1.16/introduction/getting-started/>

For further options and for configuration on other systems, see the [documentation](https://docs.influxdata.com/telegraf/v1.16/introduction/getting-started/#configure-telegraf).

Starting the Telegraph service:

```bash
sudo systemctl start telegraf
sudo systemctl status telegraf
```

## Instance

![Register a new Amazon Machine Image](/images/influxdb/2020-11-20-influxdb-aws-register-new-ami.png)

![Open AWS Marketplace](/images/influxdb/2020-11-20-influxdb-aws-open-marketplace.png)

![Select Ubuntu Image](/images/influxdb/2020-11-20-influxdb-aws-select-ubuntu.png)

![Subscribe AMI](/images/influxdb/2020-11-20-influxdb-aws-subscribe-ami.png)

- **Continue to Configuration**: ![Terms and Conditions](/images/influxdb/2020-11-20-influxdb-aws-terms-conditions.png)
- ![Configure AMI](/images/influxdb/2020-11-20-influxdb-aws-configure-ami.png)
- ![Launch Instance](/images/influxdb/2020-11-20-influxdb-aws-launch-instance.png)
- ![Instance Type](/images/influxdb/2020-11-20-influxdb-aws-instance-type.png)
- ![Launch Instance](/images/influxdb/2020-11-20-influxdb-aws-launc-instance.png)
- ![Select Key Pair](/images/influxdb/2020-11-20-influxdb-aws-select-key-pair.png)
- ![Instance Launch Screen](/images/influxdb/2020-11-20-influxdb-aws-instance-end.png)
