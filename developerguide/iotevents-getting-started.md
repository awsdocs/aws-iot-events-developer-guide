# Getting Started with the AWS IoT Events Console<a name="iotevents-getting-started"></a>

In this tutorial, you create a detector that models two states of an engine: a normal state and an over\-pressure condition\. 

When the measured pressure in the engine exceeds a certain threshold, the model transitions from the normal state to the over\-pressure state\. Then it sends an Amazon SNS message to alert a technician about the condition\. When the pressure again drops below the threshold for three consecutive pressure readings, the model returns to the normal state and sends another Amazon SNS message as a confirmation\. 

We check for three consecutive readings below the pressure threshold to eliminate possible stuttering of over\-pressure/normal messages, in case of a nonlinear recovery phase or an anomalous pressure reading\.

\(This tutorial uses the console to create the same `input` and `detector model` shown in the example at [How to Use AWS IoT Events](how-to-use-iotevents.md)\. As you follow the steps here, the information in the JSON files in that example might be helpful to view\.\)

You'll use the AWS IoT Events console to do the following\.

Define *inputs*  
To monitor your devices and processes, they must have a way to get telemetry data into AWS IoT Events\. This is done by sending messages as *inputs* to AWS IoT Events\. You can do this in several ways:  
+ Use [ BatchPutMessage](iotevents-commands.md#api-iotevents-data-BatchPutMessage)\.
+ In AWS IoT Core, you can write an [ AWS IoT Events Action](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html#iotevents-rule) rule for the AWS IoT rules engine that forwards your message data into AWS IoT Events \(identifying the input by name\)\. 
+ In AWS IoT Analytics, you can use the [ CreateDataset API](https://docs.aws.amazon.com/iotanalytics/latest/userguide/automate.html#aws-iot-analytics-automate-create-dataset) command to create a data set with `contentDeliveryRules`\. These rules specify the AWS IoT Events input where data set contents are sent automatically\.
Before your devices start sending data in this way, you must define one or more inputs\. You do this by giving each a name and specifying which fields in the incoming message data the input will monitor\.

Create a *detector model*  
You then define a *detector model* \(a model of your equipment or process\) using *states*\. For each state, you define conditional \(Boolean\) logic that evaluates the incoming inputs to detect significant events\. When an event is detected, it can change the state or trigger custom\-built or predefined actions using other AWS services\. You can define additional events that trigger actions when entering or exiting a state and, optionally, when a condition is met\.   
In this tutorial, you send an Amazon SNS message as the action when the model enters or exits a certain state\.

Monitor a Device or Process  
If you're monitoring several devices or processes, you specify a field in each input that identifies the particular device or process the input comes from\. \(See the `key` field in `CreateDetectorModel`\.\) When a new device is identified \(a new value is seen in the input field identified by the `key`\), a detector is created\. \(Each detector is an instance of the detector model\.\) Then the new detector continues responding to inputs coming from that device until its detector model is updated or deleted\.  
If you're monitoring a single process \(even if several devices or subprocesses are sending inputs\) you don't specify a unique identifying `key` field\. In this case, a single detector \(instance\) is created when the first input arrives\.

Send Messages as Inputs to Your Detector Model  
As discussed under *Define inputs*, there are several ways to send a message from a device or process as an input into an AWS IoT Events detector that don't require you to perform additional formatting on the message\. In this tutorial, you use the AWS IoT Core console to write an [ AWS IoT Events Action](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html#iotevents-rule) rule for the AWS IoT rules engine that forwards your message data into AWS IoT Events\. To do this, you identify the input by name\. Then you continue to use the AWS IoT Core console to generate some messages that are forwarded as inputs to AWS IoT Events\.

**Topics**
+ [Prerequisites](iotevents-getting-started-prereqs.md)
+ [Create an Input](iotevents-detector-input.md)
+ [Create a Detector Model](iotevents-detector-model.md)
+ [Test the Detector Model by Sending Inputs](iotevents-iot-rules-engine.md)