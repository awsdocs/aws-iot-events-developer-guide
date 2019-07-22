# Prerequisites<a name="iotevents-getting-started-prereqs"></a>

1. If you don't have an AWS account, create one\. 

   To create an AWS account: 

   Open the [AWS home page](https://aws.amazon.com/) and then choose **Create an AWS Account**\.

1. Follow the online instructions\. Part of the sign\-up procedure involves receiving a phone call and entering a PIN using your phone's keypad\.

1. Create two Amazon Simple Notification Service \(Amazon SNS\) topics\.

   This tutorial \(and the corresponding example\) assume you have created two Amazon SNS topics\. The ARNs of these topics are shown as: `arn:aws:sns:us-east-1:123456789012:underPressureAction` and `arn:aws:sns:us-east-1:123456789012:pressureClearedAction`\. Replace these values with the ARNs of Amazon SNS topics you create\. For more information, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.

   As an alternative to publishing alerts to Amazon SNS topics, you can have the detectors send MQTT messages with a topic you specify\. If you do this, you can verify that your detector model is creating instances\. You can also verify that those instances are sending alerts by using the AWS IoT Core console to subscribe to and monitor messages sent to those MQTT topics\.

1. Choose an AWS Region that supports AWS IoT Events: currently US East Northern Virginia \(us\-east\-1\), US East Ohio \(us\-east\-2\), US West Oregon \(us\-west\-2\), EU Ireland \(eu\-west\-1\), Asia Pacific Tokyo \(ap\-northeast\-1\), Asia Pacific Sydney \(ap\-southeast\-2\) and EU Frankfurt \(eu\-central\-1\)\. For help, see [ Working with the AWS Management Console](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html) in the *AWS Management Console Getting Started Guide*\.