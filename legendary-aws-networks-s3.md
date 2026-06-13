<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Access S3 from a VPC

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-s3)

**Author:** Anas Sabir  
**Email:** anassabir008@gmail.com

---

## Access S3 from a VPC

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-s3_3e1e79a2)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is a virtual private cloud and it is useful because with it we can isolate our ressources

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to provision an isolated virtual network with a public subnet, which hosted the EC2 instance I used to run CLI commands and test connection paths to Amazon S3.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was that Amazon S3 resides entirely outside the VPC boundary, meaning that standard traffic from my EC2 instance to S3 travels over the public internet by default.

### This project took me...

This project took me approximately 40 minutes to complete, including building the network, launching the instance, configuring the AWS CLI, and verifying file access.

---

## In the first part of my project...

### Step 1 - Architecture set up

In this step, I will Create a VPC from scratch and Launch an EC2 instance into my VPC

### Step 2 - Connect to my EC2 instance

In this step, I will Connect directly to your EC2 instance.

### Step 3 - Set up access keys

In this step, I will Give my EC2 instance access to your AWS environment via creating an acess key

---

## Architecture set up

I started my project by launching a vpc including NACL subnet and an ec2 instance with its security group

I also set up an S3 buckets and i uploaded two files in it

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-s3_4334d777)

---

## Running CLI commands

The AWS Command Line Interface (CLI) is a tool that allows you to manage and control your AWS services directly from your command line or terminal.

The first command I ran was 'aws s3 ls' and this command is used to list all s3 buckets inside theAWS acount that our instance has access to

The second command I ran was 'aws configure' This command is used to set up my EC2 instances credentials

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-s3_e7fa8776)

---

## Access keys

### Credentials

To set up my EC2 instance to interact with my AWS environment, I configured my credentials using aws configure to securely save my AWS Access Key ID, Secret Access Key, and default region.

Access keys are are credentials for applications and other servers to log into AWS and talk to AWS services/resources

Secret access keys are like the passwords that pairs with access keys ID (usernames)

### Best practice

Although I'm using access keys in this project, a best practice alternative is to create an IAM role with the necessary permissions and then attaching that role to your EC2 instance.

---

## In the second part of my project...

### Step 4 - Set up an S3 bucket

In this step, I will launch a S3 bucket 

### Step 5 - Connecting to my S3 bucket

In this step, I will get my EC2 instance to interact with your S3 bucket.

---

## Connecting to my S3 bucket

The first command I ran was 'aws s3 ls' and this command is used to list all s3 buckets inside theAWS acount that our instance has access to

When I ran the command aws s3 ls again, the terminal responded with a list of my s3 buckets which contains only the one i created

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-s3_4334d778)

---

## Connecting to my S3 bucket

Another CLI command I ran was 'aws s3 ls s3://nextwork-vpc-project-anassabir' which returned lists of objects in my s3 bucket nextwork-vpc-project-anassabir

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-s3_4334d779)

---

## Uploading objects to S3

To upload a new file to my bucket, I first ran the command 'sudo touch /tmp/test.txt' This command creates a file named test.txt in /tmp directory

The second command I ran was 'aws s3 cp /tmp/test.txt s3://nextwork-vpc-project-anassabir' This command will upload the test file in my s3 bucket

The third command I ran was aws s3 ls s3://nextwork-vpc-project-anassabir which validated that my file is correctly uploaded

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-s3_3e1e79a2)

---

---
