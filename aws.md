# AWS

## What is Cloud Computing?

The on-demand delivery of compute, database storage, applications and other IT resources through a cloud services platform via the internet with pay-as-you-go pricing.

It's renting somebody's computer!

### Advantages of Cloud Computing

1. You only pay for what you use(Trade capital expense for variable expense) - instead of having to invest in servers and data centres before starting you can pay when you consume resources.

2. Benefit from massive economies of scale - You will never have the same purchasing power as Amazon, it's cheaper for them to do it.

3. No guessing about capacity - You can scale as you need it instead of buying too much or too little.

4. Increase speed and agility - You can build things quickly using serverless architecture and scale as you need to.

5. Don't spend money on running data centres - focus on the product instead of server management.

6. Go global in minutes

### 3 Types of Cloud Computing

Infrastructure as a Service - You manage the server, the data centre provider will have no access to your server(EC2).

Platform as a Service - Someone else manages the underlying hardware and operating systems, you focus on applications(GoDaddy).

Software as a Service = All you worry about is the software itself and how you want to use it(GMail).

### 3 Types of Cloud Computing Deployment

Public Cloud - AWS, Azure, GCP

Hybrid - mixture of public and private

Private - You manage it in your data centre(OpenStack, VMWare).

### Areas of AWS

Compute - EC2(virtual machines) and Lambda(next level up, only worry about code)

Databases - Relational Database Service(RDS), DynamoDB(Non-relational databases)

Storage - Simple storage service(S3), Glacier

Network - VPC, Route 53(DNS service)

### AWS Global Infrastructure

An **availability zone** is basically a data centre or several data centres which are close together.

A data centre is a building filled with servers.

A **region** is a geographical area and consists of two or more availability zones. **Edge locations** are endpoints for AWS to cache content, e.g. if someone in London downloads something from New York, the first time it'll be slow but then if someone in the area downloads it after it will be cached in an edge location near London.

Choose a region based on:
- Data sovereignty laws(you might have to store data in the same country)
- Latency to end users(where are the majority of your users?)
- AWS Services - some regions get services released later

### AWS Support Packages

- **Basic** - free
- **Developer** - starts at $29 a month, scales based on usage
- **Business** - starts at $100 a month, scales based on usage
- **Enterprise** - $15000 a month, scales, you get a Technical Account Manager

### Set Up A Billing Alarm

Go into CloudWatch and then through to the Alarms section. Click on Create Alarm and choose the price when you want to have an alert if billing goes over that. You then need to create a Simple Notification Service(SNS) topic, give the alarm a name and enter an email address. You get sent an email to confirm the subscription to the topic. You can then make a name and description and then create the alarm!

### Identity and Access Management

IAM allows you to manage **users** and their **level of access** to the AWS console. It gives you centralised control of your AWS account, shared access, granular permissions, multi-factor authentication, the ability to provide temporary access to users and a customisable password rotation policy.

Terminology:

- **Users:** end users, employees, people
- **Groups:** collection of users, users of group inherit permissions
- **Roles:** can create a role and assign it to resources, e.g. virtual machine is given permission to access certain resources
- **Policies:** provides permissions for users, groups and roles, written in JSON 

In the IAM section, you can set up Two-factor authentication. In Account Settings, you can set a password policy to make your passwords more secure.

You can go to users and create a user, add them to a group, create groups. When you create a group, you add a policy and you can create your own or choose a default one. You can then add tags to the user to store metadata, data about the user.

After you've added a user, you will get an access ID(username), secret access key for access from the terminal, a password(temporary) and you can email the user their login details.

Main Points:

- IAM is global
- You can access the AWS platform via the AWS console, the command line or using a Software Development Kit.
- Root account has full admin access and is set to the original email which creates the AWS account. Should have 2FA.
- To set permissions to a group, apply a policy to the group. Users inherit group permissions.
- You can download a Credential Report in IAM, which will show info about passwords, access keys and 2-factor authentication

### S3 - Simple Storage Service

A safe place to save your files which won't change(flat files like pictures, videos). 

- It is object-based(key-value where key is name and value is data)
- Files can be 0 bytes to 5TB
- Unlimited Storage
- Files are stored in buckets(a folder in the cloud).
- Universal namespace(must be a unique name) - e.g. https://s3-eu-west-1.amazonaws.com/acloudguru

- Only suitable for objects, not OS or database.
- When you upload or modify you get a 200 HTTP Status Code.
- Read after write consistency after PUTS of new objects(can retrieve info as soon as you've uploaded it)
- Eventual consistency for overwrite PUTS and DELETES - can take time to propagate(if you try to retrieve data immediately after editing you may get the old version)

#### Types of S3

- All types of S3 have 11 9's durability.
- Standard, Infrequently Accessed(IA) - cheaper but still requires rapid access, IA One Zone, Intelligent Tiering(uses machine learning to optimise file organisation), Glacier(low cost but slow retrieval), Glacier Deep Archive(super slow) 

### CloudFront

CloudFront is Amazon's Content Delivery Network(CDN), a system of distributed servers(network) that deliver webpages and other content to a user based on geographic location of the user, the origin of the webpage and a content delivery server.

- Edge Location - Location where content is cached, separate from AWS region
- Origin - the origin of all the files the CDN will distribute. It can be an S3 bucket, an EC2 instance, an Elastic Load Balancer or Route53.
- Distribution - The name given to the CDN which consists of a collection of edge locations.
- Web distribution - typically used for websites
- RTMP - used for media streaming(being phased out).

- Edge locations are not just READ only, you can write to them too.
- Objects are cached in edge locations for the life of the TTL(Time To Live), counted in seconds.
- You can clear cached objects but you will be charged.

### EC2 - Elastic Compute Cloud

A virtual server(or servers) in the cloud.

Before, you would have to book a server, e.g. Rackspace, you order two servers and they go and set them up, connect them to the internet. You'd have to have a contract for a fixed time like 3 years.

#### EC2 Pricing Models

- **On Demand** - fixed rate by the hour or second with no commitment. Very flexible.
- **Reserved** - Provides you with capacity reservation with a discount on the hourly charge for an instance. Contract terms 1 to 3 years. There are different prices for types of instances - Standard Reserved(can't convert type of instances), Convertible Reserved Instances(can convert from READ to WRITE etc.), Scheduled Reserved Instances(match capacity if there are big spikes at certain times). 
- **Spot** - You bid whatever price you want for instance capacity to make it cheaper if you have flexible start and end times. Useful for low computing costs when time isn't important. If Spot instance is terminated by Amazon, you're not charged for the partial hour, if you terminate, you are charged for the hour.
- **Dedicated Hosts** - Physical EC2 server dedicated for your use. Can reduce costs by allowing you to use existing server-bound software licenses. Sometimes required by law, good for licensing which doesn't support cloud deployments. Purchased on demand.

There are lots of different EC2 instance types: FIGHTDRMCPXZAU

#### Amazon EBS

A virtual disk/server in the cloud - allows you to create storage volumes and attach them to EC2 instances. Once attached, you can create a file system or run a database on these volumes. There are two types:

- **SSD** 
- General Purpose SSD(GP2) - balances price and performance
- Provisioned IOPS SSD(IO1) - highest performance SSD volume for mission critical workloads
- **Magnetic** 
- Throughput Optimized HDD(ST1) - low cost HDD volume designed for frequently accessed workloads
- Cold HDD(SC1) - Lowest cost HDD volume designed for less frequently accessed workloads

#### Creating an EC2 Instance

- EC2 is a compute-based service. It is not serverless, it is a server!
- Use a private key to connect to EC2
- When you create an EC2 instance, you can choose the port(Linux SSH uses port 22, Micosoft uses Remote Desktop Protocol port 3389, HTTP port 80 and HTTPS port 443)
- A security group is basically a virtual firewall, you need to open ports in order to use them. When you set up the instance, you can let everything in with 0.0.0.0/0, you can let just one IP address in with X.X.X.X/32.
- Always design for failure, have one EC2 instance in each availability zone.

### Using Command Line

You need to log in with your access key and password, which you can download as a csv file when you first create a user. You can ssh into an EC2 to access the account from the terminal using your key value password file.

```
ssh ec2-user@100.24.45.181 -i MyNVKP.pem
aws configure // command to add credentials to config so you can do stuff
aws s3 mb s3://duncan1234-abc // make an S3 bucket
aws s3 ls // list buckets
echo 'hello' > hello.txt
aws s3 cp hello.txt s3://duncan1234-abc // copy file to a bucket
```
These commands are being run on EC2, as you are SSHing into the EC2 server to run these commands. Using the EC2 server, you create a new bucket and add a file to it.

You can cd into a .aws directory which contains your config and credentials files. In credentials, there are your access_key_id and your secret_access_key(password), which isn't very secure. If someone gets into your EC2, they can see your password. Roles helps solve this problem.

### Roles

You can create a role in IAM. In this example, the role gives access to EC2 and has an AmazonS3FullAccess policy added to it. Give the role a name and description and then you can go into EC2 and attach a role to an EC2 instance. You still use your private key to SSH into the EC2 instance, but you don't need a credentials file.
```
cd ~
rm -rf .aws // remove the aws credentials and config files so it uses the role
aws s3 ls
```
- Roles are more secure than using access IDs and secret access keys and they're easier to manage.
- You can apply roles to EC2 instances at any time and changes happen immediately.
- Roles are universal, you don't need to specify the region they are in.
