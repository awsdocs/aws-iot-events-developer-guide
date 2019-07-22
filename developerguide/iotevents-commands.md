# IoT Events Commands<a name="iotevents-commands"></a>

**Topics**
+ [BatchPutMessage](#api-iotevents-data-BatchPutMessage)
+ [BatchUpdateDetector](#api-iotevents-data-BatchUpdateDetector)
+ [CreateDetectorModel](#api-iotevents-CreateDetectorModel)
+ [CreateInput](#api-iotevents-CreateInput)
+ [DeleteDetectorModel](#api-iotevents-DeleteDetectorModel)
+ [DeleteInput](#api-iotevents-DeleteInput)
+ [DescribeDetector](#api-iotevents-data-DescribeDetector)
+ [DescribeDetectorModel](#api-iotevents-DescribeDetectorModel)
+ [DescribeInput](#api-iotevents-DescribeInput)
+ [DescribeLoggingOptions](#api-iotevents-DescribeLoggingOptions)
+ [ListDetectorModelVersions](#api-iotevents-ListDetectorModelVersions)
+ [ListDetectorModels](#api-iotevents-ListDetectorModels)
+ [ListDetectors](#api-iotevents-data-ListDetectors)
+ [ListInputs](#api-iotevents-ListInputs)
+ [ListTagsForResource](#api-iotevents-ListTagsForResource)
+ [PutLoggingOptions](#api-iotevents-PutLoggingOptions)
+ [TagResource](#api-iotevents-TagResource)
+ [UntagResource](#api-iotevents-UntagResource)
+ [UpdateDetectorModel](#api-iotevents-UpdateDetectorModel)
+ [UpdateInput](#api-iotevents-UpdateInput)

## BatchPutMessage<a name="api-iotevents-data-BatchPutMessage"></a>

Sends a set of messages to the AWS IoT Events system\. Each message payload is transformed into the input you specify \(`"inputName"`\) and ingested into any detectors that monitor that input\. If multiple messages are sent, the order in which the messages are processed isn't guaranteed\. To guarantee ordering, you must send messages one at a time and wait for a successful response\.

 **Synopsis**

```
aws iotevents-data  batch-put-message \
    --messages <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "messages": [
    {
      "messageId": "string",
      "inputName": "string",
      "payload": "blob"
    }
  ]
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `messages`  |  list  member: Message  |  The list of messages to send\. Each message has the following format: `'{ "messageId": "string", "inputName": "string", "payload": "string"}'`  | 
|   `messageId`  |  string  length\- max:64 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The ID to assign to the message\. Within each batch sent, each `"messageId"` must be unique\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the input into which the message payload is transformed\. | 
|   `payload`  |  blob |  The payload of the message\. This can be a JSON string or a Base\-64\-encoded string representing binary data \(in which case you must decode it\)\.  | 

Output

```
{
  "BatchPutMessageErrorEntries": [
    {
      "messageId": "string",
      "errorCode": "string",
      "errorMessage": "string"
    }
  ]
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `BatchPutMessageErrorEntries`  |  list  member: BatchPutMessageErrorEntry  |  A list of any errors encountered when sending the messages\. | 
|   `messageId`  |  string  length\- max:64 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The ID of the message that caused the error\. \(See the value corresponding to the `"messageId"` key in the `"message"` object\.\)  | 
|   `errorCode`  |  string |  The code associated with the error\.  enum: ResourceNotFoundException \| InvalidRequestException \| InternalFailureException \| ServiceUnavailableException \| ThrottlingException  | 
|   `errorMessage`  |  string |  More information about the error\. | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

`ThrottlingException`  
The request could not be completed due to throttling\.

## BatchUpdateDetector<a name="api-iotevents-data-BatchUpdateDetector"></a>

Updates the state, variable values, and timer settings of one or more detectors \(instances\) of a specified detector model\.

 **Synopsis**

```
aws iotevents-data  batch-update-detector \
    --detectors <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "detectors": [
    {
      "messageId": "string",
      "detectorModelName": "string",
      "keyValue": "string",
      "state": {
        "stateName": "string",
        "variables": [
          {
            "name": "string",
            "value": "string"
          }
        ],
        "timers": [
          {
            "name": "string",
            "seconds": "integer"
          }
        ]
      }
    }
  ]
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectors`  |  list  member: UpdateDetectorRequest  |  The list of detectors \(instances\) to update, along with the values to update\. | 
|   `messageId`  |  string  length\- max:64 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The ID to assign to the detector update `"message"`\. Each `"messageId"` must be unique within each batch sent\.  | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model that created the detectors \(instances\)\. | 
|   `keyValue`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\-\_:\]\+$  |  The value of the input key attribute \(identifying the device or system\) that caused the creation of this detector \(instance\)\.  | 
|   `state`  |  DetectorStateDefinition |  The new state, variable values, and timer settings of the detector \(instance\)\. | 
|   `stateName`  |  string  length\- max:128 min:1  |  The name of the new state of the detector \(instance\)\. | 
|   `variables`  |  list  member: VariableDefinition  |  The new values of the detector's variables\. Any variable whose value isn't specified is cleared\. | 
|   `name`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `timers`  |  list  member: TimerDefinition  |  The new values of the detector's timers\. Any timer whose value isn't specified is cleared, and its timeout event won't occur\.  | 
|   `name`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The new setting of the timer \(the number of seconds before the timer elapses\)\. | 

Output

```
{
  "batchUpdateDetectorErrorEntries": [
    {
      "messageId": "string",
      "errorCode": "string",
      "errorMessage": "string"
    }
  ]
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `batchUpdateDetectorErrorEntries`  |  list  member: BatchUpdateDetectorErrorEntry  |  A list of those detector updates that resulted in errors\. \(If an error is listed here, the specific update did not occur\.\)  | 
|   `messageId`  |  string  length\- max:64 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The `"messageId"` of the update request that caused the error\. \(The value of the `"messageId"` in the update request `"Detector"` object\.\)  | 
|   `errorCode`  |  string |  The code of the error\.  enum: ResourceNotFoundException \| InvalidRequestException \| InternalFailureException \| ServiceUnavailableException \| ThrottlingException  | 
|   `errorMessage`  |  string |  A message describing the error\. | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

`ThrottlingException`  
The request could not be completed due to throttling\.

## CreateDetectorModel<a name="api-iotevents-CreateDetectorModel"></a>

Creates a detector model\.

 **Synopsis**

```
aws iotevents  create-detector-model \
    --detector-model-name <value> \
    --detector-model-definition <value> \
    [--detector-model-description <value>] \
    [--key <value>] \
    --role-arn <value> \
    [--tags <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "detectorModelName": "string",
  "detectorModelDefinition": {
    "states": [
      {
        "stateName": "string",
        "onInput": {
          "events": [
            {
              "eventName": "string",
              "condition": "string",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "string",
                    "value": "string"
                  },
                  "sns": {
                    "targetArn": "string"
                  },
                  "iotTopicPublish": {
                    "mqttTopic": "string"
                  },
                  "setTimer": {
                    "timerName": "string",
                    "seconds": "integer"
                  },
                  "clearTimer": {
                    "timerName": "string"
                  },
                  "resetTimer": {
                    "timerName": "string"
                  },
                  "lambda": {
                    "functionArn": "string"
                  },
                  "iotEvents": {
                    "inputName": "string"
                  },
                  "sqs": {
                    "queueUrl": "string",
                    "useBase64": "boolean"
                  },
                  "firehose": {
                    "deliveryStreamName": "string",
                    "separator": "string"
                  }
                }
              ]
            }
          ],
          "transitionEvents": [
            {
              "eventName": "string",
              "condition": "string",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "string",
                    "value": "string"
                  },
                  "sns": {
                    "targetArn": "string"
                  },
                  "iotTopicPublish": {
                    "mqttTopic": "string"
                  },
                  "setTimer": {
                    "timerName": "string",
                    "seconds": "integer"
                  },
                  "clearTimer": {
                    "timerName": "string"
                  },
                  "resetTimer": {
                    "timerName": "string"
                  },
                  "lambda": {
                    "functionArn": "string"
                  },
                  "iotEvents": {
                    "inputName": "string"
                  },
                  "sqs": {
                    "queueUrl": "string",
                    "useBase64": "boolean"
                  },
                  "firehose": {
                    "deliveryStreamName": "string",
                    "separator": "string"
                  }
                }
              ],
              "nextState": "string"
            }
          ]
        },
        "onEnter": {
          "events": [
            {
              "eventName": "string",
              "condition": "string",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "string",
                    "value": "string"
                  },
                  "sns": {
                    "targetArn": "string"
                  },
                  "iotTopicPublish": {
                    "mqttTopic": "string"
                  },
                  "setTimer": {
                    "timerName": "string",
                    "seconds": "integer"
                  },
                  "clearTimer": {
                    "timerName": "string"
                  },
                  "resetTimer": {
                    "timerName": "string"
                  },
                  "lambda": {
                    "functionArn": "string"
                  },
                  "iotEvents": {
                    "inputName": "string"
                  },
                  "sqs": {
                    "queueUrl": "string",
                    "useBase64": "boolean"
                  },
                  "firehose": {
                    "deliveryStreamName": "string",
                    "separator": "string"
                  }
                }
              ]
            }
          ]
        },
        "onExit": {
          "events": [
            {
              "eventName": "string",
              "condition": "string",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "string",
                    "value": "string"
                  },
                  "sns": {
                    "targetArn": "string"
                  },
                  "iotTopicPublish": {
                    "mqttTopic": "string"
                  },
                  "setTimer": {
                    "timerName": "string",
                    "seconds": "integer"
                  },
                  "clearTimer": {
                    "timerName": "string"
                  },
                  "resetTimer": {
                    "timerName": "string"
                  },
                  "lambda": {
                    "functionArn": "string"
                  },
                  "iotEvents": {
                    "inputName": "string"
                  },
                  "sqs": {
                    "queueUrl": "string",
                    "useBase64": "boolean"
                  },
                  "firehose": {
                    "deliveryStreamName": "string",
                    "separator": "string"
                  }
                }
              ]
            }
          ]
        }
      }
    ],
    "initialStateName": "string"
  },
  "detectorModelDescription": "string",
  "key": "string",
  "roleArn": "string",
  "tags": [
    {
      "key": "string",
      "value": "string"
    }
  ]
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model\. | 
|   `detectorModelDefinition`  |  DetectorModelDefinition |  Information that defines how the detectors operate\. | 
|   `states`  |  list  member: State  |  Information about the states of the detector\. | 
|   `stateName`  |  string  length\- max:128 min:1  |  The name of the state\. | 
|   `onInput`  |  OnInputLifecycle |  When an input is received and the `"condition"` is TRUE, perform the specified `"actions"`\.  | 
|   `events`  |  list  member: Event  |  Specifies the actions performed when the `"condition"` evaluates to TRUE\. | 
|   `eventName`  |  string  length\- max:128  |  The name of the event\. | 
|   `condition`  |  string  length\- max:512  |  \[Optional\] The Boolean expression that when TRUE causes the `"actions"` to be performed\. If not present, the actions are performed \(=TRUE\); if the expression result is not a Boolean value, the actions are NOT performed \(=FALSE\)\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `transitionEvents`  |  list  member: TransitionEvent  |  Specifies the actions performed, and the next state entered, when a `"condition"` evaluates to TRUE\.  | 
|   `eventName`  |  string  length\- max:128  |  The name of the transition event\. | 
|   `condition`  |  string  length\- max:512  |  \[Required\] A Boolean expression that when TRUE causes the actions to be performed and the `"nextState"` to be entered\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `nextState`  |  string  length\- max:128 min:1  |  The next state to enter\. | 
|   `onEnter`  |  OnEnterLifecycle |  When entering this state, perform these `"actions"` if the `"condition"` is TRUE\.  | 
|   `events`  |  list  member: Event  |  Specifies the actions that are performed when the state is entered and the `"condition"` is TRUE\.  | 
|   `eventName`  |  string  length\- max:128  |  The name of the event\. | 
|   `condition`  |  string  length\- max:512  |  \[Optional\] The Boolean expression that when TRUE causes the `"actions"` to be performed\. If not present, the actions are performed \(=TRUE\); if the expression result is not a Boolean value, the actions are NOT performed \(=FALSE\)\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `onExit`  |  OnExitLifecycle |  When exiting this state, perform these `"actions"` if the specified `"condition"` is TRUE\.  | 
|   `events`  |  list  member: Event  |  Specifies the `"actions"` that are performed when the state is exited and the `"condition"` is TRUE\.  | 
|   `eventName`  |  string  length\- max:128  |  The name of the event\. | 
|   `condition`  |  string  length\- max:512  |  \[Optional\] The Boolean expression that when TRUE causes the `"actions"` to be performed\. If not present, the actions are performed \(=TRUE\); if the expression result is not a Boolean value, the actions are NOT performed \(=FALSE\)\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `initialStateName`  |  string  length\- max:128 min:1  |  The state that is entered at the creation of each detector \(instance\)\. | 
|   `detectorModelDescription`  |  string  length\- max:128  |  A brief description of the detector model\. | 
|   `key`  |  string  length\- max:128 min:1  pattern: ^\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\(\.\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\)\*$  |  The input attribute key used to identify a device or system to create a detector \(an instance of the detector model\) and then to route each input received to the appropriate detector \(instance\)\. This parameter uses a JSON\-path expression to specify the attribute\-value pair in the message payload of each input that is used to identify the device associated with the input\.  | 
|   `roleArn`  |  string  length\- max:2048 min:1  |  The ARN of the role that grants permission to AWS IoT Events to perform its operations\. | 
|   `tags`  |  list  member: Tag  |  Metadata that can be used to manage the detector model\. | 
|   `key`  |  string  length\- max:128 min:1  |  The tag's key\. | 
|   `value`  |  string  length\- max:256 min:0  |  The tag's value\. | 

Output

```
{
  "detectorModelConfiguration": {
    "detectorModelName": "string",
    "detectorModelVersion": "string",
    "detectorModelDescription": "string",
    "detectorModelArn": "string",
    "roleArn": "string",
    "creationTime": "timestamp",
    "lastUpdateTime": "timestamp",
    "status": "string",
    "key": "string"
  }
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelConfiguration`  |  DetectorModelConfiguration |  Information about how the detector model is configured\. | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model\. | 
|   `detectorModelVersion`  |  string  length\- max:128 min:1  |  The version of the detector model\. | 
|   `detectorModelDescription`  |  string  length\- max:128  |  A brief description of the detector model\. | 
|   `detectorModelArn`  |  string |  The ARN of the detector model\. | 
|   `roleArn`  |  string  length\- max:2048 min:1  |  The ARN of the role that grants permission to AWS IoT Events to perform its operations\. | 
|   `creationTime`  |  timestamp |  The time the detector model was created\. | 
|   `lastUpdateTime`  |  timestamp |  The time the detector model was last updated\. | 
|   `status`  |  string |  The status of the detector model\.  enum: ACTIVE \| ACTIVATING \| INACTIVE \| DEPRECATED \| DRAFT \| PAUSED \| FAILED  | 
|   `key`  |  string  length\- max:128 min:1  pattern: ^\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\(\.\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\)\*$  |  The input attribute key used to identify a device or system to create a detector \(an instance of the detector model\) and then to route each input received to the appropriate detector \(instance\)\. This parameter uses a JSON\-path expression to specify the attribute\-value pair in the message payload of each input that is used to identify the device associated with the input\.  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceInUseException`  
The resource is in use\.

`ResourceAlreadyExistsException`  
The resource already exists\.

`LimitExceededException`  
A limit was exceeded\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## CreateInput<a name="api-iotevents-CreateInput"></a>

Creates an input\.

 **Synopsis**

```
aws iotevents  create-input \
    --input-name <value> \
    [--input-description <value>] \
    --input-definition <value> \
    [--tags <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "inputName": "string",
  "inputDescription": "string",
  "inputDefinition": {
    "attributes": [
      {
        "jsonPath": "string"
      }
    ]
  },
  "tags": [
    {
      "key": "string",
      "value": "string"
    }
  ]
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name you want to give to the input\. | 
|   `inputDescription`  |  string  length\- max:128  |  A brief description of the input\. | 
|   `inputDefinition`  |  InputDefinition |  The definition of the input\. | 
|   `attributes`  |  list  member: Attribute  |  The attributes from the JSON payload that are made available by the input\. Inputs are derived from messages sent to the AWS IoT Events system using `BatchPutMessage`\. Each such message contains a JSON payload, and those attributes \(and their paired values\) specified here are available for use in the `"condition"` expressions used by detectors that monitor this input\.   | 
|   `jsonPath`  |  string  length\- max:128 min:1  pattern: ^\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\(\.\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\)\*$  |  An expression that specifies an attribute\-value pair in a JSON structure\. Use this to specify an attribute from the JSON payload that is made available by the input\. Inputs are derived from messages sent to the AWS IoT Events system \(`BatchPutMessage`\)\. Each such message contains a JSON payload, and the attribute \(and its paired value\) specified here are available for use in the `"condition"` expressions used by detectors\.  Syntax: `<field-name>.<field-name>...`  | 
|   `tags`  |  list  member: Tag  |  Metadata that can be used to manage the input\. | 
|   `key`  |  string  length\- max:128 min:1  |  The tag's key\. | 
|   `value`  |  string  length\- max:256 min:0  |  The tag's value\. | 

Output

```
{
  "inputConfiguration": {
    "inputName": "string",
    "inputDescription": "string",
    "inputArn": "string",
    "creationTime": "timestamp",
    "lastUpdateTime": "timestamp",
    "status": "string"
  }
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `inputConfiguration`  |  InputConfiguration |  Information about the configuration of the input\. | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the input\. | 
|   `inputDescription`  |  string  length\- max:128  |  A brief description of the input\. | 
|   `inputArn`  |  string |  The ARN of the input\. | 
|   `creationTime`  |  timestamp |  The time the input was created\. | 
|   `lastUpdateTime`  |  timestamp |  The last time the input was updated\. | 
|   `status`  |  string |  The status of the input\.  enum: CREATING \| UPDATING \| ACTIVE \| DELETING  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

`ResourceAlreadyExistsException`  
The resource already exists\.

## DeleteDetectorModel<a name="api-iotevents-DeleteDetectorModel"></a>

Deletes a detector model\. Any active instances of the detector model are also deleted\.

 **Synopsis**

```
aws iotevents  delete-detector-model \
    --detector-model-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "detectorModelName": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model to be deleted\. | 

Output

None

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceInUseException`  
The resource is in use\.

`ResourceNotFoundException`  
The resource was not found\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## DeleteInput<a name="api-iotevents-DeleteInput"></a>

Deletes an input\.

 **Synopsis**

```
aws iotevents  delete-input \
    --input-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "inputName": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the input to delete\. | 

Output

None

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceNotFoundException`  
The resource was not found\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

`ResourceInUseException`  
The resource is in use\.

## DescribeDetector<a name="api-iotevents-data-DescribeDetector"></a>

Returns information about the specified detector \(instance\)\.

 **Synopsis**

```
aws iotevents-data  describe-detector \
    --detector-model-name <value> \
    [--key-value <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "detectorModelName": "string",
  "keyValue": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model whose detectors \(instances\) you want information about\. | 
|   `keyValue`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\-\_:\]\+$  |  A filter used to limit results to detectors \(instances\) created because of the given key ID\. | 

Output

```
{
  "detector": {
    "detectorModelName": "string",
    "keyValue": "string",
    "detectorModelVersion": "string",
    "state": {
      "stateName": "string",
      "variables": [
        {
          "name": "string",
          "value": "string"
        }
      ],
      "timers": [
        {
          "name": "string",
          "timestamp": "timestamp"
        }
      ]
    },
    "creationTime": "timestamp",
    "lastUpdateTime": "timestamp"
  }
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detector`  |  Detector |  Information about the detector \(instance\)\. | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model that created this detector \(instance\)\. | 
|   `keyValue`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\-\_:\]\+$  |  The value of the key \(identifying the device or system\) that caused the creation of this detector \(instance\)\.  | 
|   `detectorModelVersion`  |  string  length\- max:128 min:1  |  The version of the detector model that created this detector \(instance\)\. | 
|   `state`  |  DetectorState |  The current state of the detector \(instance\)\. | 
|   `stateName`  |  string  length\- max:128 min:1  |  The name of the state\. | 
|   `variables`  |  list  member: Variable  |  The current values of the detector's variables\. | 
|   `name`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The current value of the variable\. | 
|   `timers`  |  list  member: Timer  |  The current state of the detector's timers\. | 
|   `name`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `timestamp`  |  timestamp |  The number of seconds which have elapsed on the timer\. | 
|   `creationTime`  |  timestamp |  The time the detector \(instance\) was created\. | 
|   `lastUpdateTime`  |  timestamp |  The time the detector \(instance\) was last updated\. | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceNotFoundException`  
The resource was not found\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## DescribeDetectorModel<a name="api-iotevents-DescribeDetectorModel"></a>

Describes a detector model\. If the `"version"` parameter is not specified, information about the latest version is returned\.

 **Synopsis**

```
aws iotevents  describe-detector-model \
    --detector-model-name <value> \
    [--detector-model-version <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "detectorModelName": "string",
  "detectorModelVersion": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model\. | 
|   `detectorModelVersion`  |  string  length\- max:128 min:1  |  The version of the detector model\. | 

Output

```
{
  "detectorModel": {
    "detectorModelDefinition": {
      "states": [
        {
          "stateName": "string",
          "onInput": {
            "events": [
              {
                "eventName": "string",
                "condition": "string",
                "actions": [
                  {
                    "setVariable": {
                      "variableName": "string",
                      "value": "string"
                    },
                    "sns": {
                      "targetArn": "string"
                    },
                    "iotTopicPublish": {
                      "mqttTopic": "string"
                    },
                    "setTimer": {
                      "timerName": "string",
                      "seconds": "integer"
                    },
                    "clearTimer": {
                      "timerName": "string"
                    },
                    "resetTimer": {
                      "timerName": "string"
                    },
                    "lambda": {
                      "functionArn": "string"
                    },
                    "iotEvents": {
                      "inputName": "string"
                    },
                    "sqs": {
                      "queueUrl": "string",
                      "useBase64": "boolean"
                    },
                    "firehose": {
                      "deliveryStreamName": "string",
                      "separator": "string"
                    }
                  }
                ]
              }
            ],
            "transitionEvents": [
              {
                "eventName": "string",
                "condition": "string",
                "actions": [
                  {
                    "setVariable": {
                      "variableName": "string",
                      "value": "string"
                    },
                    "sns": {
                      "targetArn": "string"
                    },
                    "iotTopicPublish": {
                      "mqttTopic": "string"
                    },
                    "setTimer": {
                      "timerName": "string",
                      "seconds": "integer"
                    },
                    "clearTimer": {
                      "timerName": "string"
                    },
                    "resetTimer": {
                      "timerName": "string"
                    },
                    "lambda": {
                      "functionArn": "string"
                    },
                    "iotEvents": {
                      "inputName": "string"
                    },
                    "sqs": {
                      "queueUrl": "string",
                      "useBase64": "boolean"
                    },
                    "firehose": {
                      "deliveryStreamName": "string",
                      "separator": "string"
                    }
                  }
                ],
                "nextState": "string"
              }
            ]
          },
          "onEnter": {
            "events": [
              {
                "eventName": "string",
                "condition": "string",
                "actions": [
                  {
                    "setVariable": {
                      "variableName": "string",
                      "value": "string"
                    },
                    "sns": {
                      "targetArn": "string"
                    },
                    "iotTopicPublish": {
                      "mqttTopic": "string"
                    },
                    "setTimer": {
                      "timerName": "string",
                      "seconds": "integer"
                    },
                    "clearTimer": {
                      "timerName": "string"
                    },
                    "resetTimer": {
                      "timerName": "string"
                    },
                    "lambda": {
                      "functionArn": "string"
                    },
                    "iotEvents": {
                      "inputName": "string"
                    },
                    "sqs": {
                      "queueUrl": "string",
                      "useBase64": "boolean"
                    },
                    "firehose": {
                      "deliveryStreamName": "string",
                      "separator": "string"
                    }
                  }
                ]
              }
            ]
          },
          "onExit": {
            "events": [
              {
                "eventName": "string",
                "condition": "string",
                "actions": [
                  {
                    "setVariable": {
                      "variableName": "string",
                      "value": "string"
                    },
                    "sns": {
                      "targetArn": "string"
                    },
                    "iotTopicPublish": {
                      "mqttTopic": "string"
                    },
                    "setTimer": {
                      "timerName": "string",
                      "seconds": "integer"
                    },
                    "clearTimer": {
                      "timerName": "string"
                    },
                    "resetTimer": {
                      "timerName": "string"
                    },
                    "lambda": {
                      "functionArn": "string"
                    },
                    "iotEvents": {
                      "inputName": "string"
                    },
                    "sqs": {
                      "queueUrl": "string",
                      "useBase64": "boolean"
                    },
                    "firehose": {
                      "deliveryStreamName": "string",
                      "separator": "string"
                    }
                  }
                ]
              }
            ]
          }
        }
      ],
      "initialStateName": "string"
    },
    "detectorModelConfiguration": {
      "detectorModelName": "string",
      "detectorModelVersion": "string",
      "detectorModelDescription": "string",
      "detectorModelArn": "string",
      "roleArn": "string",
      "creationTime": "timestamp",
      "lastUpdateTime": "timestamp",
      "status": "string",
      "key": "string"
    }
  }
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModel`  |  DetectorModel |  Information about the detector model\. | 
|   `detectorModelDefinition`  |  DetectorModelDefinition |  Information that defines how a detector operates\. | 
|   `states`  |  list  member: State  |  Information about the states of the detector\. | 
|   `stateName`  |  string  length\- max:128 min:1  |  The name of the state\. | 
|   `onInput`  |  OnInputLifecycle |  When an input is received and the `"condition"` is TRUE, perform the specified `"actions"`\.  | 
|   `events`  |  list  member: Event  |  Specifies the actions performed when the `"condition"` evaluates to TRUE\. | 
|   `eventName`  |  string  length\- max:128  |  The name of the event\. | 
|   `condition`  |  string  length\- max:512  |  \[Optional\] The Boolean expression that when TRUE causes the `"actions"` to be performed\. If not present, the actions are performed \(=TRUE\); if the expression result is not a Boolean value, the actions are NOT performed \(=FALSE\)\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `transitionEvents`  |  list  member: TransitionEvent  |  Specifies the actions performed, and the next state entered, when a `"condition"` evaluates to TRUE\.  | 
|   `eventName`  |  string  length\- max:128  |  The name of the transition event\. | 
|   `condition`  |  string  length\- max:512  |  \[Required\] A Boolean expression that when TRUE causes the actions to be performed and the `"nextState"` to be entered\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `nextState`  |  string  length\- max:128 min:1  |  The next state to enter\. | 
|   `onEnter`  |  OnEnterLifecycle |  When entering this state, perform these `"actions"` if the `"condition"` is TRUE\.  | 
|   `events`  |  list  member: Event  |  Specifies the actions that are performed when the state is entered and the `"condition"` is TRUE\.  | 
|   `eventName`  |  string  length\- max:128  |  The name of the event\. | 
|   `condition`  |  string  length\- max:512  |  \[Optional\] The Boolean expression that when TRUE causes the `"actions"` to be performed\. If not present, the actions are performed \(=TRUE\); if the expression result is not a Boolean value, the actions are NOT performed \(=FALSE\)\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `onExit`  |  OnExitLifecycle |  When exiting this state, perform these `"actions"` if the specified `"condition"` is TRUE\.  | 
|   `events`  |  list  member: Event  |  Specifies the `"actions"` that are performed when the state is exited and the `"condition"` is TRUE\.  | 
|   `eventName`  |  string  length\- max:128  |  The name of the event\. | 
|   `condition`  |  string  length\- max:512  |  \[Optional\] The Boolean expression that when TRUE causes the `"actions"` to be performed\. If not present, the actions are performed \(=TRUE\); if the expression result is not a Boolean value, the actions are NOT performed \(=FALSE\)\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `initialStateName`  |  string  length\- max:128 min:1  |  The state that is entered at the creation of each detector \(instance\)\. | 
|   `detectorModelConfiguration`  |  DetectorModelConfiguration |  Information about how the detector is configured\. | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model\. | 
|   `detectorModelVersion`  |  string  length\- max:128 min:1  |  The version of the detector model\. | 
|   `detectorModelDescription`  |  string  length\- max:128  |  A brief description of the detector model\. | 
|   `detectorModelArn`  |  string |  The ARN of the detector model\. | 
|   `roleArn`  |  string  length\- max:2048 min:1  |  The ARN of the role that grants permission to AWS IoT Events to perform its operations\. | 
|   `creationTime`  |  timestamp |  The time the detector model was created\. | 
|   `lastUpdateTime`  |  timestamp |  The time the detector model was last updated\. | 
|   `status`  |  string |  The status of the detector model\.  enum: ACTIVE \| ACTIVATING \| INACTIVE \| DEPRECATED \| DRAFT \| PAUSED \| FAILED  | 
|   `key`  |  string  length\- max:128 min:1  pattern: ^\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\(\.\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\)\*$  |  The input attribute key used to identify a device or system to create a detector \(an instance of the detector model\) and then to route each input received to the appropriate detector \(instance\)\. This parameter uses a JSON\-path expression to specify the attribute\-value pair in the message payload of each input that is used to identify the device associated with the input\.  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceNotFoundException`  
The resource was not found\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## DescribeInput<a name="api-iotevents-DescribeInput"></a>

Describes an input\.

 **Synopsis**

```
aws iotevents  describe-input \
    --input-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "inputName": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the input\. | 

Output

```
{
  "input": {
    "inputConfiguration": {
      "inputName": "string",
      "inputDescription": "string",
      "inputArn": "string",
      "creationTime": "timestamp",
      "lastUpdateTime": "timestamp",
      "status": "string"
    },
    "inputDefinition": {
      "attributes": [
        {
          "jsonPath": "string"
        }
      ]
    }
  }
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `input`  |  Input |  Information about the input\. | 
|   `inputConfiguration`  |  InputConfiguration |  Information about the configuration of an input\. | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the input\. | 
|   `inputDescription`  |  string  length\- max:128  |  A brief description of the input\. | 
|   `inputArn`  |  string |  The ARN of the input\. | 
|   `creationTime`  |  timestamp |  The time the input was created\. | 
|   `lastUpdateTime`  |  timestamp |  The last time the input was updated\. | 
|   `status`  |  string |  The status of the input\.  enum: CREATING \| UPDATING \| ACTIVE \| DELETING  | 
|   `inputDefinition`  |  InputDefinition |  The definition of the input\. | 
|   `attributes`  |  list  member: Attribute  |  The attributes from the JSON payload that are made available by the input\. Inputs are derived from messages sent to the AWS IoT Events system using `BatchPutMessage`\. Each such message contains a JSON payload, and those attributes \(and their paired values\) specified here are available for use in the `"condition"` expressions used by detectors that monitor this input\.   | 
|   `jsonPath`  |  string  length\- max:128 min:1  pattern: ^\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\(\.\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\)\*$  |  An expression that specifies an attribute\-value pair in a JSON structure\. Use this to specify an attribute from the JSON payload that is made available by the input\. Inputs are derived from messages sent to the AWS IoT Events system \(`BatchPutMessage`\)\. Each such message contains a JSON payload, and the attribute \(and its paired value\) specified here are available for use in the `"condition"` expressions used by detectors\.  Syntax: `<field-name>.<field-name>...`  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceNotFoundException`  
The resource was not found\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## DescribeLoggingOptions<a name="api-iotevents-DescribeLoggingOptions"></a>

Retrieves the current settings of the AWS IoT Events logging options\.

 **Synopsis**

```
aws iotevents  describe-logging-options  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
}
```

Output

```
{
  "loggingOptions": {
    "roleArn": "string",
    "level": "string",
    "enabled": "boolean",
    "detectorDebugOptions": [
      {
        "detectorModelName": "string",
        "keyValue": "string"
      }
    ]
  }
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `loggingOptions`  |  LoggingOptions |  The current settings of the AWS IoT Events logging options\. | 
|   `roleArn`  |  string  length\- max:2048 min:1  |  The ARN of the role that grants permission to AWS IoT Events to perform logging\. | 
|   `level`  |  string |  The logging level\.  enum: ERROR \| INFO \| DEBUG  | 
|   `enabled`  |  boolean |  If TRUE, logging is enabled for AWS IoT Events\. | 
|   `detectorDebugOptions`  |  list  member: DetectorDebugOption  |  Information that identifies those detector models and their detectors \(instances\) for which the logging level is given\.  | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model\. | 
|   `keyValue`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\-\_:\]\+$  |  The value of the input attribute key used to create the detector \(the instance of the detector model\)\.  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ResourceNotFoundException`  
The resource was not found\.

`ServiceUnavailableException`  
The service is currently unavailable\.

`UnsupportedOperationException`  
The requested operation is not supported\.

## ListDetectorModelVersions<a name="api-iotevents-ListDetectorModelVersions"></a>

Lists all the versions of a detector model\. Only the metadata associated with each detector model version is returned\.

 **Synopsis**

```
aws iotevents  list-detector-model-versions \
    --detector-model-name <value> \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "detectorModelName": "string",
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model whose versions are returned\. | 
|   `nextToken`  |  string |  The token for the next set of results\. | 
|   `maxResults`  |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. | 

Output

```
{
  "detectorModelVersionSummaries": [
    {
      "detectorModelName": "string",
      "detectorModelVersion": "string",
      "detectorModelArn": "string",
      "roleArn": "string",
      "creationTime": "timestamp",
      "lastUpdateTime": "timestamp",
      "status": "string"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelVersionSummaries`  |  list  member: DetectorModelVersionSummary  |  Summary information about the detector model versions\. | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model\. | 
|   `detectorModelVersion`  |  string  length\- max:128 min:1  |  The ID of the detector model version\. | 
|   `detectorModelArn`  |  string |  The ARN of the detector model version\. | 
|   `roleArn`  |  string  length\- max:2048 min:1  |  The ARN of the role that grants the detector model permission to perform its tasks\. | 
|   `creationTime`  |  timestamp |  The time the detector model version was created\. | 
|   `lastUpdateTime`  |  timestamp |  The last time the detector model version was updated\. | 
|   `status`  |  string |  The status of the detector model version\.  enum: ACTIVE \| ACTIVATING \| INACTIVE \| DEPRECATED \| DRAFT \| PAUSED \| FAILED  | 
|   `nextToken`  |  string |  A token to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceNotFoundException`  
The resource was not found\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## ListDetectorModels<a name="api-iotevents-ListDetectorModels"></a>

Lists the detector models you have created\. Only the metadata associated with each detector model is returned\.

 **Synopsis**

```
aws iotevents  list-detector-models \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `nextToken`  |  string |  The token for the next set of results\. | 
|   `maxResults`  |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. | 

Output

```
{
  "detectorModelSummaries": [
    {
      "detectorModelName": "string",
      "detectorModelDescription": "string",
      "creationTime": "timestamp"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelSummaries`  |  list  member: DetectorModelSummary  |  Summary information about the detector models\. | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model\. | 
|   `detectorModelDescription`  |  string  length\- max:128  |  A brief description of the detector model\. | 
|   `creationTime`  |  timestamp |  The time the detector model was created\. | 
|   `nextToken`  |  string |  A token to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## ListDetectors<a name="api-iotevents-data-ListDetectors"></a>

Lists detectors \(the instances of a detector model\)\.

 **Synopsis**

```
aws iotevents-data  list-detectors \
    --detector-model-name <value> \
    [--state-name <value>] \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "detectorModelName": "string",
  "stateName": "string",
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model whose detectors \(instances\) are listed\. | 
|   `stateName`  |  string  length\- max:128 min:1  |  A filter that limits results to those detectors \(instances\) in the given state\. | 
|   `nextToken`  |  string |  The token for the next set of results\. | 
|   `maxResults`  |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. | 

Output

```
{
  "detectorSummaries": [
    {
      "detectorModelName": "string",
      "keyValue": "string",
      "detectorModelVersion": "string",
      "state": {
        "stateName": "string"
      },
      "creationTime": "timestamp",
      "lastUpdateTime": "timestamp"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorSummaries`  |  list  member: DetectorSummary  |  A list of summary information about the detectors \(instances\)\. | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model that created this detector \(instance\)\. | 
|   `keyValue`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\-\_:\]\+$  |  The value of the key \(identifying the device or system\) that caused the creation of this detector \(instance\)\.  | 
|   `detectorModelVersion`  |  string  length\- max:128 min:1  |  The version of the detector model that created this detector \(instance\)\. | 
|   `state`  |  DetectorStateSummary |  The current state of the detector \(instance\)\. | 
|   `stateName`  |  string  length\- max:128 min:1  |  The name of the state\. | 
|   `creationTime`  |  timestamp |  The time the detector \(instance\) was created\. | 
|   `lastUpdateTime`  |  timestamp |  The time the detector \(instance\) was last updated\. | 
|   `nextToken`  |  string |  A token to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceNotFoundException`  
The resource was not found\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## ListInputs<a name="api-iotevents-ListInputs"></a>

Lists the inputs you have created\.

 **Synopsis**

```
aws iotevents  list-inputs \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `nextToken`  |  string |  The token for the next set of results\. | 
|   `maxResults`  |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. | 

Output

```
{
  "inputSummaries": [
    {
      "inputName": "string",
      "inputDescription": "string",
      "inputArn": "string",
      "creationTime": "timestamp",
      "lastUpdateTime": "timestamp",
      "status": "string"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `inputSummaries`  |  list  member: InputSummary  |  Summary information about the inputs\. | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the input\. | 
|   `inputDescription`  |  string  length\- max:128  |  A brief description of the input\. | 
|   `inputArn`  |  string |  The ARN of the input\. | 
|   `creationTime`  |  timestamp |  The time the input was created\. | 
|   `lastUpdateTime`  |  timestamp |  The last time the input was updated\. | 
|   `status`  |  string |  The status of the input\.  enum: CREATING \| UPDATING \| ACTIVE \| DELETING  | 
|   `nextToken`  |  string |  A token to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## ListTagsForResource<a name="api-iotevents-ListTagsForResource"></a>

Lists the tags \(metadata\) you have assigned to the resource\.

 **Synopsis**

```
aws iotevents  list-tags-for-resource \
    --resource-arn <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "resourceArn": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `resourceArn`  |  string  length\- max:2048 min:1  |  The ARN of the resource\. | 

Output

```
{
  "tags": [
    {
      "key": "string",
      "value": "string"
    }
  ]
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `tags`  |  list  member: Tag  |  The list of tags assigned to the resource\. | 
|   `key`  |  string  length\- max:128 min:1  |  The tag's key\. | 
|   `value`  |  string  length\- max:256 min:0  |  The tag's value\. | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceNotFoundException`  
The resource was not found\.

`ResourceInUseException`  
The resource is in use\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

## PutLoggingOptions<a name="api-iotevents-PutLoggingOptions"></a>

Sets or updates the AWS IoT Events logging options\.

If you update the value of any `"loggingOptions"` field, it takes up to one minute for the change to take effect\. Also, if you change the policy attached to the role you specified in the `"roleArn"` field \(for example, to correct an invalid policy\) it takes up to five minutes for that change to take effect\.

 **Synopsis**

```
aws iotevents  put-logging-options \
    --logging-options <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "loggingOptions": {
    "roleArn": "string",
    "level": "string",
    "enabled": "boolean",
    "detectorDebugOptions": [
      {
        "detectorModelName": "string",
        "keyValue": "string"
      }
    ]
  }
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `loggingOptions`  |  LoggingOptions |  The new values of the AWS IoT Events logging options\. | 
|   `roleArn`  |  string  length\- max:2048 min:1  |  The ARN of the role that grants permission to AWS IoT Events to perform logging\. | 
|   `level`  |  string |  The logging level\.  enum: ERROR \| INFO \| DEBUG  | 
|   `enabled`  |  boolean |  If TRUE, logging is enabled for AWS IoT Events\. | 
|   `detectorDebugOptions`  |  list  member: DetectorDebugOption  |  Information that identifies those detector models and their detectors \(instances\) for which the logging level is given\.  | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model\. | 
|   `keyValue`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\-\_:\]\+$  |  The value of the input attribute key used to create the detector \(the instance of the detector model\)\.  | 

Output

None

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

`UnsupportedOperationException`  
The requested operation is not supported\.

`ResourceInUseException`  
The resource is in use\.

## TagResource<a name="api-iotevents-TagResource"></a>

Adds to or modifies the tags of the given resource\. Tags are metadata that can be used to manage a resource\.

 **Synopsis**

```
aws iotevents  tag-resource \
    --resource-arn <value> \
    --tags <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "resourceArn": "string",
  "tags": [
    {
      "key": "string",
      "value": "string"
    }
  ]
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `resourceArn`  |  string  length\- max:2048 min:1  |  The ARN of the resource\. | 
|   `tags`  |  list  member: Tag  |  The new or modified tags for the resource\. | 
|   `key`  |  string  length\- max:128 min:1  |  The tag's key\. | 
|   `value`  |  string  length\- max:256 min:0  |  The tag's value\. | 

Output

None

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceNotFoundException`  
The resource was not found\.

`ResourceInUseException`  
The resource is in use\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`LimitExceededException`  
A limit was exceeded\.

`InternalFailureException`  
An internal failure occurred\.

## UntagResource<a name="api-iotevents-UntagResource"></a>

Removes the given tags \(metadata\) from the resource\.

 **Synopsis**

```
aws iotevents  untag-resource \
    --resource-arn <value> \
    --tag-keys <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "resourceArn": "string",
  "tagKeys": [
    "string"
  ]
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `resourceArn`  |  string  length\- max:2048 min:1  |  The ARN of the resource\. | 
|   `tagKeys`  |  list  member: TagKey  |  A list of the keys of the tags to be removed from the resource\. | 

Output

None

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceNotFoundException`  
The resource was not found\.

`ResourceInUseException`  
The resource is in use\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

## UpdateDetectorModel<a name="api-iotevents-UpdateDetectorModel"></a>

Updates a detector model\. Detectors \(instances\) spawned by the previous version are deleted and then re\-created as new inputs arrive\.

 **Synopsis**

```
aws iotevents  update-detector-model \
    --detector-model-name <value> \
    --detector-model-definition <value> \
    [--detector-model-description <value>] \
    --role-arn <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "detectorModelName": "string",
  "detectorModelDefinition": {
    "states": [
      {
        "stateName": "string",
        "onInput": {
          "events": [
            {
              "eventName": "string",
              "condition": "string",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "string",
                    "value": "string"
                  },
                  "sns": {
                    "targetArn": "string"
                  },
                  "iotTopicPublish": {
                    "mqttTopic": "string"
                  },
                  "setTimer": {
                    "timerName": "string",
                    "seconds": "integer"
                  },
                  "clearTimer": {
                    "timerName": "string"
                  },
                  "resetTimer": {
                    "timerName": "string"
                  },
                  "lambda": {
                    "functionArn": "string"
                  },
                  "iotEvents": {
                    "inputName": "string"
                  },
                  "sqs": {
                    "queueUrl": "string",
                    "useBase64": "boolean"
                  },
                  "firehose": {
                    "deliveryStreamName": "string",
                    "separator": "string"
                  }
                }
              ]
            }
          ],
          "transitionEvents": [
            {
              "eventName": "string",
              "condition": "string",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "string",
                    "value": "string"
                  },
                  "sns": {
                    "targetArn": "string"
                  },
                  "iotTopicPublish": {
                    "mqttTopic": "string"
                  },
                  "setTimer": {
                    "timerName": "string",
                    "seconds": "integer"
                  },
                  "clearTimer": {
                    "timerName": "string"
                  },
                  "resetTimer": {
                    "timerName": "string"
                  },
                  "lambda": {
                    "functionArn": "string"
                  },
                  "iotEvents": {
                    "inputName": "string"
                  },
                  "sqs": {
                    "queueUrl": "string",
                    "useBase64": "boolean"
                  },
                  "firehose": {
                    "deliveryStreamName": "string",
                    "separator": "string"
                  }
                }
              ],
              "nextState": "string"
            }
          ]
        },
        "onEnter": {
          "events": [
            {
              "eventName": "string",
              "condition": "string",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "string",
                    "value": "string"
                  },
                  "sns": {
                    "targetArn": "string"
                  },
                  "iotTopicPublish": {
                    "mqttTopic": "string"
                  },
                  "setTimer": {
                    "timerName": "string",
                    "seconds": "integer"
                  },
                  "clearTimer": {
                    "timerName": "string"
                  },
                  "resetTimer": {
                    "timerName": "string"
                  },
                  "lambda": {
                    "functionArn": "string"
                  },
                  "iotEvents": {
                    "inputName": "string"
                  },
                  "sqs": {
                    "queueUrl": "string",
                    "useBase64": "boolean"
                  },
                  "firehose": {
                    "deliveryStreamName": "string",
                    "separator": "string"
                  }
                }
              ]
            }
          ]
        },
        "onExit": {
          "events": [
            {
              "eventName": "string",
              "condition": "string",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "string",
                    "value": "string"
                  },
                  "sns": {
                    "targetArn": "string"
                  },
                  "iotTopicPublish": {
                    "mqttTopic": "string"
                  },
                  "setTimer": {
                    "timerName": "string",
                    "seconds": "integer"
                  },
                  "clearTimer": {
                    "timerName": "string"
                  },
                  "resetTimer": {
                    "timerName": "string"
                  },
                  "lambda": {
                    "functionArn": "string"
                  },
                  "iotEvents": {
                    "inputName": "string"
                  },
                  "sqs": {
                    "queueUrl": "string",
                    "useBase64": "boolean"
                  },
                  "firehose": {
                    "deliveryStreamName": "string",
                    "separator": "string"
                  }
                }
              ]
            }
          ]
        }
      }
    ],
    "initialStateName": "string"
  },
  "detectorModelDescription": "string",
  "roleArn": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model that is updated\. | 
|   `detectorModelDefinition`  |  DetectorModelDefinition |  Information that defines how a detector operates\. | 
|   `states`  |  list  member: State  |  Information about the states of the detector\. | 
|   `stateName`  |  string  length\- max:128 min:1  |  The name of the state\. | 
|   `onInput`  |  OnInputLifecycle |  When an input is received and the `"condition"` is TRUE, perform the specified `"actions"`\.  | 
|   `events`  |  list  member: Event  |  Specifies the actions performed when the `"condition"` evaluates to TRUE\. | 
|   `eventName`  |  string  length\- max:128  |  The name of the event\. | 
|   `condition`  |  string  length\- max:512  |  \[Optional\] The Boolean expression that when TRUE causes the `"actions"` to be performed\. If not present, the actions are performed \(=TRUE\); if the expression result is not a Boolean value, the actions are NOT performed \(=FALSE\)\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `transitionEvents`  |  list  member: TransitionEvent  |  Specifies the actions performed, and the next state entered, when a `"condition"` evaluates to TRUE\.  | 
|   `eventName`  |  string  length\- max:128  |  The name of the transition event\. | 
|   `condition`  |  string  length\- max:512  |  \[Required\] A Boolean expression that when TRUE causes the actions to be performed and the `"nextState"` to be entered\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `nextState`  |  string  length\- max:128 min:1  |  The next state to enter\. | 
|   `onEnter`  |  OnEnterLifecycle |  When entering this state, perform these `"actions"` if the `"condition"` is TRUE\.  | 
|   `events`  |  list  member: Event  |  Specifies the actions that are performed when the state is entered and the `"condition"` is TRUE\.  | 
|   `eventName`  |  string  length\- max:128  |  The name of the event\. | 
|   `condition`  |  string  length\- max:512  |  \[Optional\] The Boolean expression that when TRUE causes the `"actions"` to be performed\. If not present, the actions are performed \(=TRUE\); if the expression result is not a Boolean value, the actions are NOT performed \(=FALSE\)\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `onExit`  |  OnExitLifecycle |  When exiting this state, perform these `"actions"` if the specified `"condition"` is TRUE\.  | 
|   `events`  |  list  member: Event  |  Specifies the `"actions"` that are performed when the state is exited and the `"condition"` is TRUE\.  | 
|   `eventName`  |  string  length\- max:128  |  The name of the event\. | 
|   `condition`  |  string  length\- max:512  |  \[Optional\] The Boolean expression that when TRUE causes the `"actions"` to be performed\. If not present, the actions are performed \(=TRUE\); if the expression result is not a Boolean value, the actions are NOT performed \(=FALSE\)\.  | 
|   `actions`  |  list  member: Action  |  The actions to be performed\. | 
|   `setVariable`  |  SetVariableAction |  Sets a variable to a specified value\. | 
|   `variableName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the variable\. | 
|   `value`  |  string  length\- max:1024 min:1  |  The new value of the variable\. | 
|   `sns`  |  SNSTopicPublishAction |  Sends an Amazon SNS message\. | 
|   `targetArn`  |  string  length\- max:2048 min:1  |  The ARN of the Amazon SNS target where the message is sent\. | 
|   `iotTopicPublish`  |  IotTopicPublishAction |  Publishes an MQTT message with the given topic to the AWS IoT message broker\. | 
|   `mqttTopic`  |  string  length\- max:128 min:1  |  The MQTT topic of the message\. | 
|   `setTimer`  |  SetTimerAction |  Information needed to set the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer\. | 
|   `seconds`  |  integer |  The number of seconds until the timer expires\. The minimum value is 60 seconds to ensure accuracy\. | 
|   `clearTimer`  |  ClearTimerAction |  Information needed to clear the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to clear\. | 
|   `resetTimer`  |  ResetTimerAction |  Information needed to reset the timer\. | 
|   `timerName`  |  string  length\- max:128 min:1  |  The name of the timer to reset\. | 
|   `lambda`  |  LambdaAction |  Calls an AWS Lambda function, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `functionArn`  |  string  length\- max:2048 min:1  |  The ARN of the AWS Lambda function which is executed\. | 
|   `iotEvents`  |  IotEventsAction |  Sends an IoT Events input, passing in information about the detector model instance and the event which triggered the action\.  | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the AWS IoT Events input where the data is sent\. | 
|   `sqs`  |  SqsAction |  Sends information about the detector model instance and the event which triggered the action to an Amazon SQS queue\.  | 
|   `queueUrl`  |  string |  The URL of the Amazon SQS queue where the data is written\. | 
|   `useBase64`  |  boolean |  Set this to TRUE if you want the data to be Base\-64 encoded before it is written to the queue\. Otherwise, set this to FALSE\.  | 
|   `firehose`  |  FirehoseAction |  Sends information about the detector model instance and the event which triggered the action to a Kinesis Data Firehose delivery stream\.  | 
|   `deliveryStreamName`  |  string |  The name of the Kinesis Data Firehose delivery stream where the data is written\. | 
|   `separator`  |  string  pattern: \(\[ \]\)\|\( \)\|\(,\)   |  A character separator that is used to separate records written to the Kinesis Data Firehose delivery stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.  | 
|   `initialStateName`  |  string  length\- max:128 min:1  |  The state that is entered at the creation of each detector \(instance\)\. | 
|   `detectorModelDescription`  |  string  length\- max:128  |  A brief description of the detector model\. | 
|   `roleArn`  |  string  length\- max:2048 min:1  |  The ARN of the role that grants permission to AWS IoT Events to perform its operations\. | 

Output

```
{
  "detectorModelConfiguration": {
    "detectorModelName": "string",
    "detectorModelVersion": "string",
    "detectorModelDescription": "string",
    "detectorModelArn": "string",
    "roleArn": "string",
    "creationTime": "timestamp",
    "lastUpdateTime": "timestamp",
    "status": "string",
    "key": "string"
  }
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `detectorModelConfiguration`  |  DetectorModelConfiguration |  Information about how the detector model is configured\. | 
|   `detectorModelName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z0\-9\_\-\]\+$  |  The name of the detector model\. | 
|   `detectorModelVersion`  |  string  length\- max:128 min:1  |  The version of the detector model\. | 
|   `detectorModelDescription`  |  string  length\- max:128  |  A brief description of the detector model\. | 
|   `detectorModelArn`  |  string |  The ARN of the detector model\. | 
|   `roleArn`  |  string  length\- max:2048 min:1  |  The ARN of the role that grants permission to AWS IoT Events to perform its operations\. | 
|   `creationTime`  |  timestamp |  The time the detector model was created\. | 
|   `lastUpdateTime`  |  timestamp |  The time the detector model was last updated\. | 
|   `status`  |  string |  The status of the detector model\.  enum: ACTIVE \| ACTIVATING \| INACTIVE \| DEPRECATED \| DRAFT \| PAUSED \| FAILED  | 
|   `key`  |  string  length\- max:128 min:1  pattern: ^\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\(\.\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\)\*$  |  The input attribute key used to identify a device or system to create a detector \(an instance of the detector model\) and then to route each input received to the appropriate detector \(instance\)\. This parameter uses a JSON\-path expression to specify the attribute\-value pair in the message payload of each input that is used to identify the device associated with the input\.  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ResourceInUseException`  
The resource is in use\.

`ResourceNotFoundException`  
The resource was not found\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

## UpdateInput<a name="api-iotevents-UpdateInput"></a>

Updates an input\.

 **Synopsis**

```
aws iotevents  update-input \
    --input-name <value> \
    [--input-description <value>] \
    --input-definition <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "inputName": "string",
  "inputDescription": "string",
  "inputDefinition": {
    "attributes": [
      {
        "jsonPath": "string"
      }
    ]
  }
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the input you want to update\. | 
|   `inputDescription`  |  string  length\- max:128  |  A brief description of the input\. | 
|   `inputDefinition`  |  InputDefinition |  The definition of the input\. | 
|   `attributes`  |  list  member: Attribute  |  The attributes from the JSON payload that are made available by the input\. Inputs are derived from messages sent to the AWS IoT Events system using `BatchPutMessage`\. Each such message contains a JSON payload, and those attributes \(and their paired values\) specified here are available for use in the `"condition"` expressions used by detectors that monitor this input\.   | 
|   `jsonPath`  |  string  length\- max:128 min:1  pattern: ^\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\(\.\(\(`\[w\- \]\+`\)\|\(\[w\-\]\+\)\)\)\*$  |  An expression that specifies an attribute\-value pair in a JSON structure\. Use this to specify an attribute from the JSON payload that is made available by the input\. Inputs are derived from messages sent to the AWS IoT Events system \(`BatchPutMessage`\)\. Each such message contains a JSON payload, and the attribute \(and its paired value\) specified here are available for use in the `"condition"` expressions used by detectors\.  Syntax: `<field-name>.<field-name>...`  | 

Output

```
{
  "inputConfiguration": {
    "inputName": "string",
    "inputDescription": "string",
    "inputArn": "string",
    "creationTime": "timestamp",
    "lastUpdateTime": "timestamp",
    "status": "string"
  }
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|   `inputConfiguration`  |  InputConfiguration |  Information about the configuration of the input\. | 
|   `inputName`  |  string  length\- max:128 min:1  pattern: ^\[a\-zA\-Z\]\[a\-zA\-Z0\-9\_\]\*$  |  The name of the input\. | 
|   `inputDescription`  |  string  length\- max:128  |  A brief description of the input\. | 
|   `inputArn`  |  string |  The ARN of the input\. | 
|   `creationTime`  |  timestamp |  The time the input was created\. | 
|   `lastUpdateTime`  |  timestamp |  The last time the input was updated\. | 
|   `status`  |  string |  The status of the input\.  enum: CREATING \| UPDATING \| ACTIVE \| DELETING  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`ThrottlingException`  
The request could not be completed due to throttling\.

`ResourceNotFoundException`  
The resource was not found\.

`InternalFailureException`  
An internal failure occurred\.

`ServiceUnavailableException`  
The service is currently unavailable\.

`ResourceInUseException`  
The resource is in use\.