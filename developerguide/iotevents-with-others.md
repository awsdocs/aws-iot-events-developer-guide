# Using AWS IoT Events with Other Services<a name="iotevents-with-others"></a>

AWS IoT Events works with other AWS IoT services to receive telemetry data and to trigger actions\.

**Receive inputs**  
AWS IoT Events receives its inputs, in the form of JSON payloads, from many sources\. Each input can be acted on alone or combined with other inputs to detect more complex events\.   
AWS IoT Events enables you to gather inputs from devices or processes in the following ways:  
+ Use the AWS IoT Events [ BatchPutMessage](iotevents-commands.md#api-iotevents-data-BatchPutMessage) API \(using the AWS CLI or an AWS SDK\)\.
+ In AWS IoT Core, write an [ AWS IoT Events Action](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html#iotevents-rule) rule for the AWS IoT rules engine that forwards your message data into AWS IoT Events\. \(You must identify the input by name\.\)
+ In AWS IoT Analytics, use the [ CreateDataset](https://docs.aws.amazon.com/iotanalytics/latest/userguide/automate.html#aws-iot-analytics-automate-create-dataset) API command to create a data set with `contentDeliveryRules` that specify the AWS IoT Events input where you want data set contents to be sent automatically\.
+ Define an `iotEvents` action in an AWS IoT Events detector model's `onInput`, `onExit` or `transitionEvents` event\. Information about the detector model instance and the event which triggered the action are fed back in to the system as an input with the name you specify\.
Each of these services enables you to send inputs directly to AWS IoT Events without the need for additional formatting\.

**Event Actions**  
AWS IoT Events provides these types of actions for an event:  
+ Send a message using the [ Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) \(`actions.sns`\)\.
+ Send a message using the [ AWS IoT Message Broker](https://docs.aws.amazon.com/iot/latest/developerguide/iot-message-broker.html) \(`actions.iotTopicPublish`\)\.
+ Set the value of a variable \(`actions.setVariable`\)\.
+ Set, reset, or clear a timer \(`actions.setTimer|resetTimer|clearTimer`\)\.
+ Call a [Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) function, passing in information about the detector model instance and the event which triggered the action \(`actions.lambda`\)\.
+ Send an input back in to AWS IoT Events, passing information about the detector model instance and the event which triggered the action \(`actions.iotEvents`\)\.
+ Send detector model instance and triggering event information to an [ Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) queue \(`actions.sqs`\)\.
+ Send detector model instance and triggering event information to a [Kinesis Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html) stream \(`actions.firehose`\)\.
In turn, Amazon SNS and the AWS IoT Rules Engine also allow you to trigger an AWS Lambda function\. This makes it possible to take actions using other services such as Amazon Connect, or even a company enterprise resource planning \(ERP\) application\.

**Note**  
You shouldn't use AWS IoT Events to process high\-frequency data streams directly\. These types of data streams should be processed in other services, such as [Amazon Kinesis](https://docs.aws.amazon.com/kinesis/index.html), where you can complete initial analysis and then send the results to AWS IoT Events as an input to a detector\. 