# Packer templates to create AWS EC2 AMI

Collection of Packer templates used for various infrastructure layers.

## INFRASTRUCTURE AS CODE
[Packer](https://www.packer.io/docs/index.html) is easy to use and automates the creation of any type of machine image. Out of the box Packer comes with support to build images for Amazon EC2, CloudStack, DigitalOcean, Docker, Google Compute Engine, Microsoft Azure, QEMU, VirtualBox, VMware, and more.

## CentOS Official image
The custom image is based on [CentOS Official AMI image](https://wiki.centos.org/Cloud/AWS)

## command to build a new AMI image to AWS
``` bash
export AWS_ACCESS_KEY_ID=''
export AWS_SECRET_ACCESS_KEY=''
PACKER_LOG=1 packer build packer_aws_ebs_conf.json
```
## env config bits

Most of the templates in here require some env vars.  Take a look at
[packer_aws_instance_conf_example.json](./packer_aws_instance_conf_example.json) for an example.

## ERROR

1. No subnets found
Error launching source instance: MissingInput: No subnets found for the default VPC 'vpc-5f619a3a'. Please specify a subnet.

###Solution:
```
# The following 2 lines don't appear in the tutorial.
# But I had to add them because it said this source AMI
# must be launched inside a VPC.
# t2.micro instance type can only run in a VPC environment.

"vpc_id": "vpc-98765432",
"subnet_id": "subnet-12345678"
```
More easier way is to use m3.medium instance type, a bit expensive but it run everything quicker and you don't need to setup VPC/Security Groups at all.

2. SSH timeout
TCP connection to SSH ip/port failed: dial tcp 52.221.211.79:22: i/o timeout

###Solution:
Need to add the following line to connect using public IP:
"associate_public_ip_address": "true"

