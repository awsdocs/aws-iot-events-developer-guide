# Creating an alarm model<a name="create-alarm-model"></a>

You can use AWS IoT Events alarms to monitor your data and get notified when a threshold is breached\. Alarms provide parameters that you use to create or configure an alarm model\. You can use the AWS IoT Events console or AWS IoT Events API to create or configure the alarm model\. When you configure the alarm model, changes take effect as new data arrives\.

## Requirements<a name="create-alarm-model-requirements"></a>

The following requirements apply when you create an alarm model\.
+ You can create an alarm model to monitor an input attribute in AWS IoT Events or an asset property in AWS IoT SiteWise\.
  + If you choose to monitor an input attribute in AWS IoT Events, do the following before you create the alarm model: 
    +  **Step 1:** Read the overview in [create an input](https://docs.aws.amazon.com/iotevents/latest/developerguide/create-input-overview.html)\.
    +  **Step 2:** Read the instructions to [ create an input in the navigation pane](https://docs.aws.amazon.com/iotevents/latest/developerguide/create-input-for-models.html)\. 

    
  + If you choose to monitor an asset property, you must [create an asset model](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/create-asset-models.html) in AWS IoT SiteWise before you create the alarm model\.
+ You must have an IAM role that allows your alarm to perform actions and access AWS resources\. For more information, see [Setting up permissions for AWS IoT Events](https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-start.html)\.
+ All the AWS resources that this tutorial uses must be in the same AWS Region\.