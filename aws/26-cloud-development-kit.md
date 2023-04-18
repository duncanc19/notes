# 26: Cloud Development Kit (CDK)

### Overview

Define your cloud infrastructure using programming languages (JS, Python, Java, .NET)

- The Cloud Development Kit is built on top of CloudFormation (using yaml files to build your infrastructure)
- It contains high level components called **constructs**
- The code is compiled into a CloudFormation template (into yaml/JSON)
- It means that you can deploy infrastructure and application code together, which is useful for Lambda, Docker containers in Elastic Container Service

![](screenshot-2022-12-06-at-09-56-55-lbc1rtaq.png)


### CDK vs SAM (Serverless Application Model)

SAM:
- is serverless focused
- lets you write your template declaratively in YAML or JSON
- is useful for quickly setting up Lambdas
- leverages CloudFormation

CDK:
- supports all AWS services
- allows you to write infrastructure in a programming language
- leverages CloudFormation (as it generates YAML)

## Hands On

Creating a pipeline where when images are uploaded into S3, a Lambda is triggered which passes the images to Rekognition, which uses AI to find objects in the images and uploads a row to a DynamoDB table for each image.

### steps.sh

```bash
# 1. install the CDK
sudo npm install -g aws-cdk

# directory name must be cdk-app/ to go with the rest of the tutorial, changing it will cause an error
mkdir cdk-app
cd cdk-app/

# initialize the application
cdk init --language javascript

# verify it works correctly
cdk ls

# install the necessary packages
npm install @aws-cdk/aws-s3 @aws-cdk/aws-iam @aws-cdk/aws-lambda @aws-cdk/aws-lambda-event-sources @aws-cdk/aws-dynamodb


# 2. copy the content of cdk-app-stack.js into lib/cdk-app-stack.js


# 3. setup the Lambda function
mkdir lambda && touch index.py

# 4. bootstrap the CDK application
cdk bootstrap

# 5. (optional) synthesize as a CloudFormation template
cdk synth


# 6. deploy the CDK stack - you'll be able to see the new stack in CloudFormation in the Console
cdk deploy

# 7. empty the s3 bucket
# 8. destroy the stack
cdk destroy
```

### cdk-app-stack.js

```js
const cdk = require("@aws-cdk/core");
const s3 = require("@aws-cdk/aws-s3");
const iam = require("@aws-cdk/aws-iam");
const lambda = require("@aws-cdk/aws-lambda");
const lambdaEventSource = require("@aws-cdk/aws-lambda-event-sources");
const dynamodb = require("@aws-cdk/aws-dynamodb");

const imageBucket = "cdk-rekn-imagebucket";

class CdkAppStack extends cdk.Stack {
    /**
     *
     * @param {cdk.Construct} scope
     * @param {string} id
     * @param {cdk.StackProps=} props
     */
    constructor(scope, id, props) {
        super(scope, id, props);

        // The code that defines your stack goes here

        // ========================================
        // Bucket for storing images
        // ========================================
        const bucket = new s3.Bucket(this, imageBucket, {
            removalPolicy: cdk.RemovalPolicy.DESTROY,
        });
        new cdk.CfnOutput(this, "Bucket", { value: bucket.bucketName });

        // ========================================
        // Role for AWS Lambda
        // ========================================
        const role = new iam.Role(this, "cdk-rekn-lambdarole", {
            assumedBy: new iam.ServicePrincipal("lambda.amazonaws.com"),
        });
        role.addToPolicy(
            new iam.PolicyStatement({
                effect: iam.Effect.ALLOW,
                actions: [
                    "rekognition:*",
                    "logs:CreateLogGroup",
                    "logs:CreateLogStream",
                    "logs:PutLogEvents",
                ],
                resources: ["*"],
            })
        );

        // ========================================
        // DynamoDB table for storing image labels
        // ========================================
        const table = new dynamodb.Table(this, "cdk-rekn-imagetable", {
            partitionKey: { name: "Image", type: dynamodb.AttributeType.STRING },
            removalPolicy: cdk.RemovalPolicy.DESTROY,
        });
        new cdk.CfnOutput(this, "Table", { value: table.tableName });

        // ========================================
        // AWS Lambda function
        // ========================================
        const lambdaFn = new lambda.Function(this, "cdk-rekn-function", {
            code: lambda.AssetCode.fromAsset("lambda"),
            runtime: lambda.Runtime.PYTHON_3_8,
            handler: "index.handler",
            role: role,
            environment: {
                TABLE: table.tableName,
                BUCKET: bucket.bucketName,
            },
        });
        lambdaFn.addEventSource(
            new lambdaEventSource.S3EventSource(bucket, {
                events: [s3.EventType.OBJECT_CREATED],
            })
        );

        bucket.grantReadWrite(lambdaFn);
        table.grantFullAccess(lambdaFn);
    }
}

module.exports = { CdkAppStack };
```


