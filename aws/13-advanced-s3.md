# 13: Advanced Amazon S3


### S3 Lifecycle Rules

Transition Actions:
- Configure objects to transition to another storage class
- E.g. move objects to standard Infrequent Access after 60 days, move to Glacier after 6 momths

Expiration Actions:
- Access log files can be set to delete after 365 days
- Can use to delete old versions and incomplete multi-part uploads

Rules can be created for a prefix, e.g. ```s3://my-bucket/mp3/*``` or for objects with certain tags


There are options to have lifecycle rules for moving or expiring current or non-current versions. 

S3 Analytics gives you advice for how to put together lifecycle rules - generates report but only recommends Standard and Standard IA, not One-Zone IA or Glacier



### S3 Event Notifications

- S3:ObjectCreated 
- S3:ObjectRemoved

You can filter events based on object names and respond to them by using other AWS services

**Use case:** 
- Generate thumbnails of images uploaded to S3 by responding to events

- You can respond using Lambda, SNS or SQS or EventBridge (which is the newest and has more filtering options)

In the properties section of the bucket, you can create event notifications and then choose where to send them onto, or you can enable EventBridge where all events will be stored so you can do some more in depth stuff.


### S3 Baseline Performance

Automatically scales to high request rates, latency 100-200ms

Your application can acheive at least 3500 PUT/COPY/PASTE/DELETE requests and 5500 GET/HEAD requests per second per prefix in a bucket


#### Multi-part upload 

- Parallelises uploads to speed up transfers (by breaking the file into parts).
- Must use for files above 5GB, but recommended for files bigger than 100MB.

#### S3 Transfer Acceleration

- Increase transfer speed by transferring to an Edge Location which forwards data onto the S3 bucket
- Compatible with multi-part upload


#### Reading files efficiently - S3 Byte-range Fetches

- Parallelise GETs by requesting specific byte ranges
- Better resilience in case of failure


#### S3 Select and Glacier Select

- With CSVs in S3, you can use S3 Select to run simple SQL queries on the server-side so that there is less network transfer
- Retrieve less data using SQL by performing server-side filtering