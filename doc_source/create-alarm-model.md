# Creating an alarm model<a name="create-alarm-model"></a>

You can use AWS IoT Events alarms to monitor your data and get notified when a threshold is breached\. Alarms provide parameters that you use to create or configure an alarm model\. You can use the AWS IoT Events console or AWS IoT Events API to create or configure the alarm model\. When you configure the alarm model, changes take effect as new data arrives\.

## Requirements<a name="create-alarm-model-requirements"></a>

The following requirements apply when you create an alarm model\.
+ You can create an alarm model to monitor an input attribute in AWS IoT Events or an asset property in AWS IoT SiteWise\.
  + If you choose to monitor an input attribute, you must [create an input](https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-detector-input.html) in AWS IoT Events before you create the alarm model\.

    For example, you can use the `sensorData.temperature` attribute in the following AWS IoT Events input:

    ```
    {
        "input": {
            "inputConfiguration": {
                "inputName": "TemperatureInput",
                "inputDescription": "Temperature readings from a sensor",
                "inputArn": "arn:aws:iotevents:us-east-1:123456789012:input/TemperatureInput",
                "creationTime": "2020-11-29T20:24:07.161000-08:00",
                "lastUpdateTime": "2020-11-29T20:24:07.161000-08:00",
                "status": "ACTIVE"
            },
            "inputDefinition": {
                "attributes": [
                    {
                        "jsonPath": "sensorData.temperature"
                    }
                ]
            }
        }
    }
    ```
  + If you choose to monitor an asset property, you must [create an asset model](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/create-asset-models.html) in AWS IoT SiteWise before you create the alarm model\.
+ You must have an IAM role that allows your alarm to perform actions and access AWS resources\. For more information, see [Setting up permissions for AWS IoT Events](https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-start.html)\.
+ All the AWS resources that this tutorial uses must be in the same AWS Region\.