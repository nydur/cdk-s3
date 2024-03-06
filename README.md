# 2.16 Create S3 Bucket with AWS CDK

Sample repository for assignment.

---

Code to deploy S3 Bucket is within `cdk-s3-stack.ts`

```typescript
import * as cdk from 'aws-cdk-lib';
import { Construct } from 'constructs';
import { Code, Function, Runtime } from 'aws-cdk-lib/aws-lambda';
import { join } from 'path';
import * as s3 from 'aws-cdk-lib/aws-s3'

export class CdkS3Stack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    const handler = new Function(this, 'lambdafors3',{
      runtime : Runtime.NODEJS_LATEST,
      timeout : cdk.Duration.seconds(30),
      handler : 'app.handler',
      code : Code.fromAsset(join(__dirname,'../lambda')),
      environment:{
        REGION_NAME:"ap-southeast-1",
        THUMBNAIL_SIZE:"128"
      }
    });

    const news3Bucket = new s3.Bucket(this, 'test-bucket',{
      removalPolicy : cdk.RemovalPolicy.DESTROY,
      autoDeleteObjects : true
    });
      
    news3Bucket.grantReadWrite(handler);

  }
}
```

Run `cdk init app --language=typescript` to initialise the empty folder, which will then automatically create the necessary files for deployment

Run `cdk synth` to prepare the deployment template. Similar to `terraform plan`

Run `cdk deploy` to deploy the infrastructure. Similar to `terraform apply`

Run `cdk destroy` to destroy the created infrastructure, when successfully deployed

**CDK is an alternative IaC tool to launch AWS infrastructure only.**
