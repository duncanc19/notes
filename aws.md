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
