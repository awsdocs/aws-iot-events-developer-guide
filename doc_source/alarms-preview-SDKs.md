# Alarms preview AWS CLI and AWS SDKs<a name="alarms-preview-SDKs"></a>

Because the alarms feature is in preview, you must download the alarms preview and AWS SDKs from the following links\.

The following AWS SDKs are supported by AWS IoT Events:
+ [AWS IoT Events alarms preview AWS SDK for Go](https://aws-iot-events.s3.amazonaws.com/sdk/AWSIoTEventsAlarmsPreviewGoSDK.zip)
+ [AWS IoT Events alarms preview AWS SDK for Java](https://aws-iot-events.s3.amazonaws.com/sdk/AWSIoTEventsAlarmsPreviewJavaSDK.zip)
+ [AWS IoT Events alarms preview AWS SDK for JavaScript in Node\.js](https://aws-iot-events.s3.amazonaws.com/sdk/AWSIoTEventsAlarmsPreviewNodeSDK.zip)
+ [AWS IoT Events alarms preview AWS SDK for Python \(Boto3\)](https://aws-iot-events.s3.amazonaws.com/sdk/AWSIoTEventsAlarmsPreviewPythonSDK.zip)

The following AWS SDKs are supported by AWS IoT Events Data:
+ [AWS IoT Events Data alarms preview AWS SDK for Go](https://aws-iot-events.s3.amazonaws.com/sdk/AWSIoTEventsDataAlarmsPreviewGoSDK.zip)
+ [AWS IoT Events Data alarms preview AWS SDK for Java](https://aws-iot-events.s3.amazonaws.com/sdk/AWSIoTEventsDataAlarmsPreviewJavaSDK.zip)
+ [AWS IoT Events Data alarms preview AWS SDK for JavaScript in Node\.js](https://aws-iot-events.s3.amazonaws.com/sdk/AWSIoTEventsDataAlarmsPreviewNodeSDK.zip)
+ [AWS IoT Events Data alarms preview AWS SDK for Python \(Boto3\)](https://aws-iot-events.s3.amazonaws.com/sdk/AWSIoTEventsDataAlarmsPreviewPythonSDK.zip)

## Configuring the AWS CLI<a name="alarms-preview-cli"></a>

The following shows you how to enable AWS IoT Events alarms commands in the AWS CLI\.

1. <a name="install-cli"></a>In your device's terminal, run `aws --version` to check if you installed the AWS CLI\. For more information, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) in the *AWS Command Line Interface User Guide*\.

1. Download the [iotevents\-2018\-07\-27\.normal\.json](https://aws-iot-events.s3.amazonaws.com/cli/iotevents-2018-07-27.normal.json) file\.

1. Download the [iotevents\-data\-2018\-10\-23\.normal\.json](https://aws-iot-events.s3.amazonaws.com/cli/iotevents-data-2018-10-23.normal.json) file to the same folder\.

1. Navigate to the directory where you downloaded the JSON files\.

1. In your device's terminal, run the following command\.

   ```
   aws configure add-model --service-name iotevents-alarms --service-model file://iotevents-2018-07-27.normal.json
   ```

   The `aws iotevents-alarms` AWS CLI commands are now available\.

1. Run the following command\.

   ```
   aws configure add-model --service-name iotevents-data-alarms --service-model file://iotevents-data-2018-10-23.normal.json
   ```

   The `aws iotevents-data-alarms` AWS CLI commands are now available\.

[test links](https://aws-iot-events.s3.amazonaws.com/sdk/AWSIoTEventsAlarmsPreviewGoSDK.zip)

For more information, see the [AWS IoT Events API Reference](https://docs.aws.amazon.com/iotevents/latest/apireference/)\.