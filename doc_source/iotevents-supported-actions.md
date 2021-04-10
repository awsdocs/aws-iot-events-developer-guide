# Supported actions<a name="iotevents-supported-actions"></a>

AWS IoT Events can trigger actions when it detects a specified event or transition event\. You can define built\-in actions to use a timer or set a variable, or send data to other AWS resources\.

**Note**  
When you define an action in a detector model, you can use expressions for parameters that are string data type\. For more information, see [Expressions](https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-expressions.html)\.

<a name="build-in-actions-intro"></a>AWS IoT Events supports the following actions that let you use a timer or set a variable:<a name="build-in-actions"></a>
+ [`setTimer`](built-in-actions.md#iotevents-set-timer) to create a timer\.
+ [`resetTimer`](built-in-actions.md#iotevents-reset-timer) to reset the timer\.
+ [`clearTimer`](built-in-actions.md#iotevents-clear-timer) to delete the timer\.
+ [`setVariable`](built-in-actions.md#iotevents-set-variable) to create a variable\.

<a name="work-with-aws-services-intro"></a>AWS IoT Events supports the following actions that let you work with AWS services: <a name="work-with-aws-services"></a>
+ [`iotTopicPublish`](iotevents-other-aws-services.md#iotevents-iotcore) to publish a message on an MQTT topic\.
+ [`iotEvents`](iotevents-other-aws-services.md#iotevents-iteinput) to send data to AWS IoT Events as an input value\.
+ [`iotSiteWise`](iotevents-other-aws-services.md#iotevents-iotsitewise) to send data to an asset property in AWS IoT SiteWise\.
+ [`dynamoDB`](iotevents-other-aws-services.md#iotevents-dynamodb) to send data to an Amazon DynamoDB table\.
+ [`dynamoDBv2`](iotevents-other-aws-services.md#iotevents-dynamodbv2) to send data to an Amazon DynamoDB table\.
+ [`firehose`](iotevents-other-aws-services.md#iotevents-firehose) to send data to an Amazon Kinesis Data Firehose stream\.
+ [`lambda`](iotevents-other-aws-services.md#iotevents-lambda) to invoke an AWS Lambda function\.
+ [`sns`](iotevents-other-aws-services.md#iotevents-sns) to send data as a push notification\.
+ [`sqs`](iotevents-other-aws-services.md#iotevents-sqs) to send data to an Amazon SQS queue\.