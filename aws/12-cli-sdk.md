# 12: AWS CLI, SDK, IAM roles and policies

### IAM roles and policies

You can choose from selected policies or you can generate your own, either by using the visual editor or writing JSON
You can also use the policy generator page but it’s quite similar to the visual editor which you access on the same page


### Ways to check permissions

Sometimes we may want to check whether something (a policy/user/role/group) has permission to do something. But sometimes we don’t want to actually run the commands as they may be expensive, e.g. creating an EC2 instance

### Policy Simulator

Page where you can test whether a policy/role/user/group has permission to do something

### CLI Dry Runs

Some AWS CLI commands contain a —dry-run flag to simulate API calls without actually running them

``` aws ec2 run-instances --dry-run --image-id ami-123132 --instance-type t2.micro```

No permission - will print out 'An error occurred' message and say you don't have permission

Permission - will still print out 'An error occurred' but followed by 'Request would have succeeded, but DryRun flag is set'  



### AWS CLI STS Decode

When you run API calls and they fail, you get a long error message. You can decode the error message to find out more information - it will output it as a JSON object. 

``` aws sts decode-authorization-message --encoded-message ```

You need to have the STS permission to decode for this to work. The formatting isn't great so you can echo it and then copy it into a code editor to make it more readable



### EC2 Instance Metadata

http://169.254.169.254/latest/meta-data/

You can access it from within your EC2 instance

Metadata - info about the EC2 instance
Userdata - launch script of the EC2 instance

You can do a curl command (query url)

Will list what's within it, in the list, if there is a slash it's a directory, if not it's a value


```curl http://169.254.169.254/latest/meta-data/instance-id```

```curl http://169.254.169.254/latest/meta-data/iam/security-credentials/name-of-role```

With all this metadata, you can see things such as the IAM role credentials, instance id


### AWS CLI Profiles

To manage having multiple AWS accounts, you can set up profiles. 

Within the `~/.aws folder`, you can see config (setting defaults for things like region) and credentials files.

Create a new profile by using configure with the profile flag:

```aws configure --profile my-other-aws-account```

Set access and secret access key and defaults, same as with the default credentials/config.

When you run commands, you can add the profile flag to specify which credentials to use. Without the flag, it will run with the default profile.

```aws s3 ls --profile my-other-aws-account```



### MFA with the CLI

To use MFA with the CLI, you create a temporary session.

```aws sts get-session-token --serial-number arn-of-mfa-device --token-code code --duration-seconds 3600```

You can find the arn of the MFA device in the IAM user. When you run the command, a JSON object of credentials is returned.

You then can create a CLI profile like above. 
You also need to add the session token to the ~/.aws/credentials file.

```aws_session_token = entire-token```


### AWS SDK

Software Development Kit - accessing AWS directly from your code
AWS CLI uses the Python SDK


### AWS Limits

- API Rate Limits - Limit of calls per second for API calls, e.g. DescribeInstances API for EC2 has a limit of 100 calls per second
- For intermittent errors, implement Exponential Backoff
- For consistent errors, request an API throttling limit increase

#### Exponential Backoff

- If you get a ThrottlingException intermittently, use exponential backoff
- Retry mechanism already included in SDK API calls, must implement if you're using the AWS API directly
- Should only implement retries on 5xx server errors, as these are related to throttling
- Must not implement on 4xx errors
- Doubling the amount of time you wait for the API to respond, to reduce the number of calls to the server

#### Service Quotas

You can request service limit increases by opening a ticket, or do it programatically by using Service Quotas API



### AWS Credentials Provider Chain

#### CLI Credentials Provider Chain

The CLI looks for credentials in this order:

1. Command line options --region, --output, --profile
2. Environment variables - AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN
3. CLI credentials file - ~/.aws/credentials (set by using ```aws configure```)
4. CLI config file - ~/.aws/config
5. Container credentials - for ECS tasks
6. Instance profile credentials - for EC2 Instance Profiles

#### SDK Credentials Provider Chain (Java example)

1. Java system properties
2. Environment variables
3. Credentials
4. ECS container credentials
5. Instance profile credentials


#### AWS Credentials best practices

- Never store your AWS credentials in your code
- Inherit your credentials from the credentials chain
- Working within AWS, use IAM roles
- Working from outside, use environment variables/named profiles


### AWS Signature v4 Signing

- When you call the AWS API, you sign the request using your AWS credentials
- Using SDK or CLI, the HTTP requests are signed for you
- If you're making HTTP calls directly, you need to sign the requests yourself using Signature v4 (Very complicated, better off using SDKs)

You can sign using an HTTP Header or a query string (you can see this when you open a file from S3 in the browser).