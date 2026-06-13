<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Monitoring with Flow Logs

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-monitoring)

**Author:** Anas Sabir  
**Email:** anassabir008@gmail.com

---

## VPC Monitoring with Flow Logs

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-monitoring_3e1e79a1)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is virtual private cloud and it is useful because it isolite my ressources from the public

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to set up two isolated virtual networks (VPCs) with custom CIDR blocks and connected them securely using a VPC Peering connection. I then enabled VPC Flow Logs on my subnets to capture, track, and monitor all network traffic passing between them.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was logs insights

### This project took me...

This project took me 30 mins withouts the poses

---

## In the first part of my project...

### Step 1 - Set up VPCs

We are creating two separate VPCs with different CIDR blocks to prepare for a peering connection and simulate network traffic.

### Step 2 - Launch EC2 instances

In this step, I will launch two EC2 instances in each subnet of each VPC

### Step 3 - Set up Logs

In this step, I will set up VPC Flow Logs to start monitoring network traffic and i will set up a storage space for the Flow Logs.

### Step 4 - Set IAM permissions for Logs

In this step, I will give  VPC Flow Logs the permission to write logs and send them to CloudWatch

---

## Multi-VPC Architecture

I started my project by launching two VPCs and a one public subnet for each

The CIDR blocks for VPCs 1 and 2 are different (unique) They have to be unique because we want to connect between them via a peering connection so to avoid overlapping we need unique CIDR blocks

### I also launched EC2 instances in each subnet

My EC2 instances' security groups allow all ssh trafics from anywhere in order to connect to the instance and as a choise i allowed the ICMP protocole from the the other VPC to test connectivity via a ping

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-monitoring_e7fa8775)

---

## Logs

Logs are digital records of events, errors, and transactions that occur within computer systems, helping administrators monitor activity and troubleshoot issues.

Log groups are containers in Amazon CloudWatch that share the same retention, monitoring, and access control settings for associated log streams.

### I also set up a flow log for VPC 1

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-monitoring_e8398869)

---

## IAM Policy and Roles

I created an IAM policy To grant the VPC Flow Logs service permission to publish captured network traffic logs directly to my Amazon CloudWatch log group.

I also created an IAM role because its required in the logs group set up and its a set of policies for a service

A custom trust policy is a specific type of policy that defines who or which AWS service is allowed to assume (use) an IAM role.
How it differs from an IAM policy: Standard IAM policies define what actions can be performed. In contrast, a trust policy defines who is allowed to perform those actions.

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-monitoring_4334d777)

---

## In the second part of my project...

### Step 5 - Ping testing and troubleshooting

In this step, I will Get Instance 1 to send test messages to Instance 2 via ping

### Step 6 - Set up a peering connection

In this step, I will Set up a connection link between your VPCs.

### Step 7 - Analyze flow logs

In this step, I will Review the flow logs recorded aboout VPC 1's public subnet and Analyse the flow logs to get some tasty insights

---

## Connectivity troubleshooting

My first ping test between my EC2 instances had no replies, which means the ping messages doesnt reach the dest or the response doesnt reach the source

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-monitoring_99d4ba42)

Receiving ping replies from the public IPv4 address means Instance 2 is correctly configured to respond to ping requests, and Instance 1 can actually communicate with Instance 2 if it traffic goes across the public internet!

---

## Connectivity troubleshooting

Looking at VPC 1's route table, I identified that the ping test with Instance 2's private address failed because there is no route from my vpc to the other vpc via peering connection

### To solve this, I set up a peering connection between my VPCs

I also updated both VPCs' route tables so that i can add the route to the other VPC

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-monitoring_7316a13d)

---

## Connectivity troubleshooting

I received ping replies from Instance 2's private IP address! This means there is a VPC peering connection

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-monitoring_4ec7821f)

---

## Analyzing flow logs

Flow logs tell us about ip adress source and port source also destination port and ip adress the prtocole used the amount of data sent and is it accepted or not the eni , aws acount id

For example, the flow log I've captured tells us that my ec2 in myVPC1 sent to the ec2 in the second vpc a data that is accepted

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-monitoring_d116818e)

---

## Logs Insights

Logs Insights  is an interactive, fully managed query tool used to search, analyze, and visualize logs data in real-time

The query "Top 10 byte transfers by source and destination IP addresses" is all about discovering the top 10 biggest data transfers between IP addresses in my network

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-monitoring_3e1e79a1)

---

---
