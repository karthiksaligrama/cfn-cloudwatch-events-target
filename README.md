
# cfn-cloudwatch-events-target



## Purpose

AWS CloudFormation does not support AWS CloudWatch Events Targets. This is a Lambda-backed custom resource to add support for [AWS CloudWatch Events Targets](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchEvents.html) to CloudFormation.

[This package on NPM](https://www.npmjs.com/package/cfn-cloudwatch-events-target)  
[This package on GitHub](https://www.github.com/andrew-templeton/cfn-cloudwatch-events-target)


## Implementation

This Lambda makes use of the Lambda-Backed CloudFormation Custom Resource flow module, `cfn-lambda` ([GitHub](https://github.com/andrew-templeton/cfn-lambda) / [NPM](https://www.npmjs.com/package/cfn-lambda)).


## Usage

  See [`./example.template.json`](./example.template.json) for a sample CloudFormation template. The example uses `Condition` statements, `Parameters`, and dynamic `ServiceToken` generation fully.

  The example template also depends on another Custom Resource, [`cfn-cloudwatch-events-rule`](https://github.com/andrew-templeton/cfn-cloudwatch-events-rule). Make sure to install it via `npm run cfn-lambda-deploy` as described below before trying to use this example template.


    "ResourceLogicalIdInResourcesObject": {
      "Type": "Type": "Custom::CloudWatchEventsTarget",
      "Properties": {
        "ServiceToken": "arn:aws:lambda:<cfn-region-id>:<your-account-id>:function:<this-deployed-lambda-name>",

        "Input": "{\"foo\": \"bar\"}", /* Static JSON to pass to TargetArn
                                      Will override InputPath */
        "InputPath": "$.json.path", /* JSONPath expression to extract from event
                                    Omitting both this and Input
                                    defaults to "$" (whole object) */
        "RuleArn": "SOME_ARN", /* required, update requires replacement */
        "TargetArn": "SOME_ARN" /* required */
      }
    }



## Installation of the Resource Service Lambda

#### Using the Provided Instant Install Script

The way that takes 10 seconds...

    # Have aws CLI installed + permissions for IAM and Lamdba
    $ npm run cfn-lambda-deploy


You will have this resource installed in every supported Region globally!


#### Using the AWS Console

... And the way more difficult way.

*IMPORTANT*: With this method, you must install this custom service Lambda in each AWS Region in which you want CloudFormation to be able to access the `CloudWatchEventsTarget` custom resource!

1. Go to the AWS Lambda Console Create Function view:
  - [`us-east-1` / N. Virginia](https://console.aws.amazon.com/lambda/home?region=us-east-1#/create?step=2)
  - [`us-west-2` / Oregon](https://console.aws.amazon.com/lambda/home?region=us-west-2#/create?step=2)
  - [`eu-west-1` / Ireland](https://console.aws.amazon.com/lambda/home?region=eu-west-1#/create?step=2)
  - [`ap-northeast-1` / Tokyo](https://console.aws.amazon.com/lambda/home?region=ap-northeast-1#/create?step=2)
2. Zip this repository into `/tmp/CloudWatchEventsTarget.zip`

    `$ cd $REPO_ROOT && zip -r /tmp/CloudWatchEventsTarget.zip;`

3. Enter a name in the Name blank. I suggest: `CfnLambdaResouce-CloudWatchEventsTarget`
4. Enter a Description (optional).
5. Toggle Code Entry Type to "Upload a .ZIP file"
6. Click "Upload", navigate to and select `/tmp/CloudWatchEventsTarget.zip`
7. Set the Timeout under Advanced Settings to 10 sec
8. Click the Role dropdown then click "Basic Execution Role". This will pop out a new window.
9. Select IAM Role, then select option "Create a new IAM Role"
10. Name the role `lambda_cfn_elasticsearch_domain` (or something descriptive)
11. Click "View Policy Document", click "Edit" on the right, then hit "OK"
12. Copy and paste the [`./execution-policy.json`](./execution-policy.json) document.
13. Hit "Allow". The window will close. Go back to the first window if you are not already there.
14. Click "Create Function". Finally, done! Now go to [Usage](#usage) or see [the example template](./example.template.json). Next time, stick to the instant deploy script.


#### Miscellaneous

##### Collaboration & Requests

Submit pull requests or Tweet [@ayetempleton](https://twitter.com/ayetempleton) if you want to get involved with roadmap as well, or if you want to do this for a living :)


##### License

[MIT](./License)


##### Want More CloudFormation?

Work is (extremely) active, published here:  
[Andrew's NPM Account](https://www.npmjs.com/~andrew-templeton)
