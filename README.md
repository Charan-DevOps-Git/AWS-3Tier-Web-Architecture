# AWS Three Tier Web Architecture

## Description: 
This workshop is a hands-on walk through of a three-tier web architecture in AWS. We will be manually creating the necessary network, security, app, and database components and configurations in order to run this architecture in an available and scalable manner.

## Architecture Overview
![AWS Architecture](images/Architecture.png)

In this architecture, a public-facing Application Load Balancer forwards client traffic to our web tier EC2 instances. The web tier is running Nginx webservers that are configured to serve a React.js website and redirects our API calls to the application tier’s internal facing load balancer. The internal facing load balancer then forwards that traffic to the application tier, which is written in Node.js. The application tier manipulates data in an Aurora MySQL multi-AZ database and returns it to our web tier. Load balancing, health checks and autoscaling groups are created at each layer to maintain the availability of this architecture.

# AWS PROJECT
---

# Creating 3 Tier Architecture & Integrating Other AWS Resources

## Step 1: Download Code from GitHub in Your Local System

- `https://github.com/Charan-DevOps-Git/AWS-3Tier-Web-Architecture.git`

## Step 2: Create Two S3 Buckets
- Create one S3 bucket for storing web-server & app-server code.
- Upload the code to your S3 from your local system.
- Create another S3 bucket for VPC flow logs.

![S3 Buckets](images/S3buckets.jpg)

## Step 3: Create IAM Role with Policies
- S3 read only.
- SSM managed instance core.

![IAM Policy](images/IAM_Role.jpg)

## Step 4: Create VPC, Subnets, IGW, NAT-GW, RT
- Enable auto-assign public IP for web-tier public subnets.
- Create flow logs for VPC & use the S3 bucket created above.

![VPC](images/VPC_arch.jpg)

## Step 5: Create Security Groups
1. **External-Load-Balancer-SG** --> HTTP (80): 0.0.0.0/0.
2. **Web-Tier-SG** --> HTTP --> Ext-LB-SG.
3. **Internal-Load-Balancer-SG** --> HTTP --> Web-Tier-SG.
4. **App-Tier-SG** --> Port 4000 --> Internal-LB-SG.
5. **DB-Tier-SG** --> MySQL (3306) --> App-Tier-SG.

![SecurityGroups](images/ec2-sg.jpg)

## Step 6: Create DB Subnet Group & RDS
- Create DB subnet group.
- Create RDS - Multi-AZ.
- Place them in DB subnet group created above.

![RDS](images/RDS.jpg)

## Step 7: Create Test App Server, Install Packages, Test Connections
- [Test App-Server Commands](https://github.com/Charan-DevOps-Git/AWS-3Tier-Web-Architecture/blob/main/app-server-commands.md)
- Create AMI.
- Create launch template using AMI.
- Create target group.
- Create internal load balancer.
- Create autoscaling group.
- Edit `nginx.conf` file in local system by adding Internal-LB-DNS(line no:58) & upload the file in S3.

## Step 8: Create Test Web Server, Install Packages (Nginx, Node.js (React)), Test Connections
- [Test Web-Server Commands](https://github.com/Charan-DevOps-Git/AWS-3Tier-Web-Architecture/blob/main/web-server-commands.md)
- Create AMI.
- Create launch template using AMI.
- Create target group.
- Create external load balancer.
- Create autoscaling group.

![EC2Instances](images/EC2-Instances.jpg)

## Step 9: Add External-ALB-DNS Record in Route 53

## Step 10: Create CloudWatch Alarms Along with SNS

![CloudWatch](images/CloudWatch_alarms.jpg)

![SNSTopics](images/SNSTopics.jpg)

## Step 11: Create CloudTrail

![CloudTrail](images/CloudTrail_logs.jpg)

## Outputs:

![AppConnected](images/appConnected.jpg)

![WebBuild](images/web-server-buid-success.jpg)

![ELB_Output](images/external-lb-app-output.jpg)

## Step 12: Deleting the Entire Infrastructure
---
- Delete CloudFront.
- Delete CloudWatch alarms.
- Delete records from Route 53.
- Delete load balancers, target groups, ASG, launch templates.
- Delete security group.
- Delete NAT gateway (it will take 5 mins).
- Release elastic IP.
- Delete VPC.
- Delete RDS subnet group, RDS.
---

