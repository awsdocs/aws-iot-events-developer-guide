# Simple Step\-by\-Step Example<a name="iotevents-simple-example"></a>

In this example, we use the AWS IoT Events APIs \(via CLI commands\) to create a detector that models two states of an engine: a normal state and an over\-pressure condition\.

When the measured pressure in the engine exceeds a certain threshold, the model transitions to the over\-pressure state and sends an Amazon Simple Notification Service \(Amazon SNS\) message to alert a technician to the condition\. When the pressure drops below the threshold for three consecutive pressure readings, the model returns to the normal state and sends another Amazon SNS message as a confirmation that the condition has cleared\. We require three consecutive readings below the pressure threshold to eliminate possible stuttering of over\-pressure/normal messages in case of a nonlinear recovery phase or a one\-off anomalous recovery reading\.

The following is an overview of the steps to create the detector\.

Create *inputs\.*  
To monitor your devices and processes, they must have a way to get telemetry data into AWS IoT Events\. This is done by sending messages as *inputs* to AWS IoT Events\. You can do this in several ways:  
+ Use [ BatchPutMessage](iotevents-commands.md#api-iotevents-data-BatchPutMessage)\.
+ In AWS IoT Core, write an [ AWS IoT Events Action](/iot/latest/developerguide/iot-rule-actions.html#iotevents-rule) rule for the AWS IoT rules engine that forwards your message data into AWS IoT Events \(identifying the input by name\)\. 
+ In AWS IoT Analytics, use the [ CreateDataset API](https://docs.aws.amazon.com/iotanalytics/latest/userguide/automate.html#aws-iot-analytics-automate-create-dataset) command to create a data set with `contentDeliveryRules` that specify the AWS IoT Events input, where data set contents are sent automatically\.
Before your devices start sending data in this way, you must create one or more inputs by giving each a name and specifying the fields in the incoming message data that the input will monitor\.

Create a *detector model*\.  
You create a *detector model* \(a model of your equipment or process\) using *states*\. For each state, you define conditional \(Boolean\) logic that evaluates the incoming inputs to detect significant events\. When an event is detected, it can change the state or trigger custom\-built or predefined actions using other AWS services\. You can define additional events that trigger actions when entering or exiting a state and, optionally, when a condition is met\.

Monitor several devices or processes\.  
If you're monitoring several devices or processes, you specify a field in each input that identifies the particular device or process the input comes from\. \(See the `key` field in `CreateDetectorModel`\.\) When a new device is identified \(a new value is seen in the input field identified by the `key`\), a detector instance is created\. Then the new detector instance continues to respond to inputs coming from that device until its detector model is updated or deleted\.

Or, monitor a single device or process\.  
If you're monitoring a single process \(even if several devices or subprocesses are sending inputs\), you don't specify a unique identifying `key` field\. In this case, a single detector \(instance\) is created when the first input arrives\.

Send messages from your devices or processes as inputs to your detector model\.  
We described the several ways to send a message from a device or process as an input into an AWS IoT Events detector in *inputs*\. After you created the inputs and build the detector model, you're ready to start sending data\.  
When you create a detector model, or update an existing one, it takes several minutes before the new or updated detector model begins receiving messages and creating detectors \(instances\)\. If the detector model is updated, during this time you might continue to see behavior based on the previous version\.

**Topics**
+ [How do you know if you need one instance of a detector or several?](#how-to-use-iotevents-instances)
+ [Create an Input](#iotevents-create-input)
+ [Create a Detector Model](#iotevents-create-detector)
+ [Send Messages as Inputs to a Detector](#iotevents-batch-put-messages)

## How do you know if you need one instance of a detector or several?<a name="how-to-use-iotevents-instances"></a>

You can decide how many instances you need by asking yourself what it is that you're interested in knowing\. Let's say you want to know what the weather is like today\. Is it raining \(state\)? Do you need to take an umbrella \(action\)? You can have a sensor that reports the temperature, another that reports the humidity, and others that report the barometric pressure, wind speed and direction, and precipitation\. But you must monitor all these sensors together to determine the state of the weather \(rain, snow, overcast, sunny\) and the appropriate action to take \(grab an umbrella or apply sunscreen\)\. In spite of the number of sensors, you want one detector instance to monitor the state of the weather and inform you which action to take\.

But if you're the weather forecaster for your region, you might have multiple instances of such sensor arrays, situated at different locations throughout the region\. People at each location need to know what the weather is like in that location\. In this case, you need multiple instances of your detector\. The data reported by each sensor in each location must include a field that you have designated as the `key` field\. This field allows AWS IoT Events to create a detector instance for the area, and then to continue to route this information to that detector instance as it continues to arrive\. No more ruined hair\-do's or sunburned noses\!

Essentially, if you have one situation \(one process or one location\) to monitor, you need just one detector instance\. If you have many situations \(locations, processes\) to monitor, you need multiple detector instances\.

## Create an Input<a name="iotevents-create-input"></a>

As an example, suppose your devices send messages with the following format\.

```
{
  "motorid": "Fulton-A32",
  "sensorData": {
    "pressure": 23,
    "temperature": 47
  }
}
```

You can create an input to capture the `"pressure"` data and the `"motorid"` \(that identifies the specific device that sent the message\) using the following AWS CLI command\.

```
aws iotevents create-input  --cli-input-json file://pressureInput.json 
```

The file `"pressureInput.json"` contains the following\.

```
{
  "inputName": "PressureInput",
  "inputDescription": "Pressure readings from a motor",
  "inputDefinition": {
    "attributes": [
      { "jsonPath": "sensorData.pressure" },
      { "jsonPath": "motorid" }
    ]
  }
}
```

## Create a Detector Model<a name="iotevents-create-detector"></a>

In [Create an Input](#iotevents-create-input), you created an `input` based on a message that reports pressure data from a motor\. To continue with the example, here is a detector model that responds to an over\-pressure event in a motor\.

You create two states: "`Normal`", and "`Dangerous`"\. Each detector \(instance\) enters the "`Normal`" state when it's created\. \(It is created when an input with a unique value for the `key` "`motorid`" arrives\.\)

If the detector instance receives a pressure reading of 70 or greater, it enters the "`Dangerous`" state and sends an Amazon SNS message as a warning\. If the pressure readings return to normal \(less than 70\) for three consecutive inputs, the detector returns to the "`Normal`" state and sends another Amazon SNS message as an all clear\.

This example detector model assumes you have created two Amazon SNS topics whose Amazon Resource Names \(ARNs\) are shown in the definition as `"targetArn": "arn:aws:sns:us-east-1:123456789012:underPressureAction"` and `"targetArn": "arn:aws:sns:us-east-1:123456789012:pressureClearedAction"`\. For more information, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) and, more specifically, the documentation of the [CreateTopic](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html) API command\.

This example also assumes you have created an AWS Identity and Access Management \(IAM\) role with appropriate permissions\. The ARN of this role is shown in the detector model definition as `"roleArn": "arn:aws:iam::123456789012:role/IoTEventsRole"`\. Follow the steps in [Setting Up Permissions for AWS IoT Events](iotevents-start.md#iotevents-permissions) to create this role, then copy the ARN of the role in the appropriate place in the detector model definition\.

You can create the detector model using the following AWS CLI command\.

```
aws iotevents create-detector-model  --cli-input-json file://motorDetectorModel.json
```

The file `"motorDetectorModel.json"` contains the following\.

```
{
  "detectorModelName": "motorDetectorModel",
  "detectorModelDefinition": {
    "states": [
      {
        "stateName": "Normal",
        "onEnter": {
          "events": [
            {
              "eventName": "init",
              "condition": "true",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "pressureThresholdBreached",
                    "value": "0"
                  }
                }
              ]
            }
          ]
        },
        "onInput": {
          "transitionEvents": [
            {
              "eventName": "Overpressurized",
              "condition": "$input.PressureInput.sensorData.pressure > 70",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "pressureThresholdBreached",
                    "value": "$variable.pressureThresholdBreached + 3"
                  }
                }
              ],
              "nextState": "Dangerous"
            }
          ]
        }
      }, 
      {
        "stateName": "Dangerous",
        "onEnter": {
          "events": [
            {
              "eventName": "Pressure Threshold Breached",
              "condition": "$variable.pressureThresholdBreached > 1",
              "actions": [
                {
                  "sns": {
                    "targetArn": "arn:aws:sns:us-east-1:123456789012:underPressureAction"
                  }
                }
              ]
            }
          ]
        },
        "onInput": {
          "events": [
            {
              "eventName": "Overpressurized",
              "condition": "$input.PressureInput.sensorData.pressure > 70",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "pressureThresholdBreached",
                    "value": "3"
                  }
                }
              ]
            },
            {
              "eventName": "Pressure Okay",
              "condition": "$input.PressureInput.sensorData.pressure <= 70",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "pressureThresholdBreached",
                    "value": "$variable.pressureThresholdBreached - 1"
                  }
                }
              ]
            }
          ],
          "transitionEvents": [
            {
              "eventName": "BackToNormal",
              "condition": "$input.PressureInput.sensorData.pressure <= 70 && $variable.pressureThresholdBreached <= 1",
              "nextState": "Normal"
            }
          ]
        },
        "onExit": {
          "events": [
            {
              "eventName": "Normal Pressure Restored",
              "condition": "true",
              "actions": [
                {
                  "sns": {
                    "targetArn": "arn:aws:sns:us-east-1:123456789012:pressureClearedAction"
                  }
                }
              ]
            }
          ]
        }
      }
    ],
    "initialStateName": "Normal"
  },
  "key" : "motorid",
  "roleArn": "arn:aws:iam::123456789012:role/IoTEventsRole"
}
```

## Send Messages as Inputs to a Detector<a name="iotevents-batch-put-messages"></a>

In the previous section you created a `detector model` that responds to an over\-pressure event in a motor \(see [Create a Detector Model](#iotevents-create-detector)\)\. To complete the example, you send messages from a "device" \(in this case a computer with the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) installed\) as inputs to the detector\. Each input \(message\) contains a payload as defined previously \(see [Create an Input](#iotevents-create-input)\)\.

**Note**  
When you create a detector model, or update an existing one, it takes several minutes before the new or updated detector model begins to receive messages and create detectors \(instances\)\. If you update the detector model, during this time you might continue to see behavior based on the previous version\.

Use the following AWS CLI command to send a message with data that breaches the threshold\.

```
aws iotevents-data batch-put-message --cli-input-json file://highPressureMessage.json 
```

The file "`highPressureMessage.json`" contains the following\.

```
{
  "messages": [
    {
      "messageId": "00001",
      "inputName": "PressureInput",
      "payload": "{\"motorid\": \"Fulton-A32\", \"sensorData\": {\"pressure\": 80, \"temperature\": 39} }"
    }
  ]
}
```

You must change the `messageId` in each message sent\. If you don't change it, the AWS IoT Events system de\-duplicates the messages\. \(AWS IoT Events will ignore a message if it has the same `messageID` as another message that was sent within the last five minutes\.\)

At this point, a detector \(instance\) is created to monitor events for the motor `"Fulton-A32"`\. This detector enters the `"Normal" `state when it's created\. But because we sent a pressure value above the threshold, it immediately transitions to the `"Dangerous"` state\. As it does so, the detector sends a message to the Amazon SNS endpoint whose ARN is `"arn:aws:sns:us-east-1:123456789012:underPressureAction"`\.

Now run the following AWS CLI command to send a message with data that is beneath the pressure threshold\.

```
aws iotevents-data batch-put-message --cli-input-json file://normalPressureMessage.json 
```

Here the file `"normalPressureMessage.json"` contains the following\.

```
{
  "messages": [
    {
      "messageId": "00002",
      "inputName": "PressureInput",
      "payload": "{\"motorid\": \"Fulton-A32\", \"sensorData\": {\"pressure\": 60, \"temperature\": 29} }"
    }
  ]
}
```

You must change the `messageId` in the file each time you invoke the `BatchPutMessage` command within a five\-minute period\. Send the message two more times\. After the message is sent three times, the detector \(instance\) for the motor "`Fulton-A32`" sends a message to the Amazon SNS endpoint `"arn:aws:sns:us-east-1:123456789012:pressureClearedAction"`, then reenters the `"Normal"` state\.

It is also worth noting that \(1\),you can send multiple messages at one time with `BatchPutMessage`, however \(2\), the order in which these messages are processed isn't guaranteed\. To guarantee messages \(inputs\) are processed in order, send them one at a time and wait for a successful response each time the API is called\.

Here are example SNS message payloads created by the detector model example described in this section\.

**on event "Pressure Threshold Breached"**

```
IoT> {
  "eventTime":1558129816420,
  "payload":{
    "actionExecutionId":"5d7444df-a655-3587-a609-dbd7a0f55267",
    "detector":{
      "detectorModelName":"motorDetectorModel",
      "keyValue":"Fulton-A32",
      "detectorModelVersion":"1"
    },
    "eventTriggerDetails":{
      "inputName":"PressureInput",
      "messageId":"00001",
      "triggerType":"Message"
    },
    "state":{
      "stateName":"Dangerous",
      "variables":{
        "pressureThresholdBreached":3
      },
      "timers":{}
    }
  },
  "eventName":"Pressure Threshold Breached"
}
```

**on event "Normal Pressure Restored"**

```
IoT> {
  "eventTime":1558129925568,
  "payload":{
    "actionExecutionId":"7e25fd38-2533-303d-899f-c979792a12cb",
    "detector":{
      "detectorModelName":"motorDetectorModel",
      "keyValue":"Fulton-A32",
      "detectorModelVersion":"1"
    },
    "eventTriggerDetails":{
      "inputName":"PressureInput",
      "messageId":"00004",
      "triggerType":"Message"
    },
    "state":{
      "stateName":"Dangerous",
      "variables":{
        "pressureThresholdBreached":0
      },
      "timers":{}
    }
  },
  "eventName":"Normal Pressure Restored"
}
```

If you have defined any timers, their current state will also be shown in the SNS message payloads\.

Notice that these message payloads contain information about the state of the detector \(instance\) at the time the message was sent \(that is, at the time the SNS action was executed\)\. Similar information about the state of a detector is available by invoking the [ DescribeDetector](iotevents-commands.md#api-iotevents-data-DescribeDetector) API command\.