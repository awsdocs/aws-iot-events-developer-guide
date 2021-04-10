# Creating an alarm model \(console\)<a name="create-alarm-model-console"></a>

The following shows you how to create an alarm model to monitor an AWS IoT Events attribute in the AWS IoT Events console\.

1. Sign in to the [AWS IoT Events console](https://console.aws.amazon.com/iotevents/)\.

1. In the navigation pane, choose **Alarm models**

1. On the **Alarm models** page, choose **Create alarm model**\.

1. In the **Alarm model details** section, do the following:

   1. Enter a unique name\.

   1. \(Optional\) Enter a description\.

1. In the **Alarm target** section, do the following:
**Important**  
If you choose **AWS IoT SiteWise asset property**, you must have created an asset model in AWS IoT SiteWise\.

   1. Choose **AWS IoT Events input attribute**\.

   1. Choose the input\.

   1. Choose the input attribute\. This input attribute is used as a key to create the alarm\. AWS IoT Events routes inputs associated with this key to the alarm\.

1. In the **Threshold definitions** section, you define when the alarm is invoked\. Do the following:

   1. For **Input attribute**, choose the input attribute that you want to monitor\.

      This value is on the left side of the comparison operator\.

   1. For **Operator**, choose the comparison operator\.

      You can choose from the following options: 
      + **< greater than**
      + **>= greater than or equal to**
      + **< less than**
      + **<= less than or equal to**
      + **= equal to**
      + **\!= not equal to**

   1. For **Value**, enter a value or choose an attribute in AWS IoT Events inputs\.

      This value is on the right side of the comparison operator\.

   1. For **Severity**, enter a non\-negative integer that reflects the severity level of the alarm\.

1. \(Optional\) In the **Notification settings** sections, you configure notification settings for the alarm\.

   AWS IoT Events uses an AWS Lambda function to manage alarm notifications\. Follow the [instructions](https://docs.aws.amazon.com/iotevents/latest/developerguide/lambda-support.html) to create a Lambda function for your alarm\.

   Do the following:

   1. For **Function name**, choose the Lambda function that manages alarm notifications\.

   1. 
      + For **Sender**, choose the email address that sends notifications\.
      + Choose **Add sender** to add more email addresses to your sender list\. If you use the Lambda function provided by AWS IoT Events, you must verify the sender email address in Amazon SES\. For more information, see [Using the Lambda function provided by AWS IoT Events](use-alarm-notifications.md)\.

   1. You can add up to 10 notifications\. For **Notification 1**, do the following:
      + For **Recipient**, choose the recipient\.
      + Choose **Add new user** to add more users to your recipient list\. You must add new users to your AWS SSO store before you can add them to your alarm model\. For more information, see [Managing recipients](sso-authorization-recipients.md)\.
      + For **Protocol**, choose from the following options:
        + **Text & email** \- The alarm sends an SMS notification and an email notification\.
        + **Email** \- The alarm sends an email notification\.
        + **Text** \- The alarm sends an SMS notification\.

   1. For **Additional custom message**, you might enter a message that describes what the alarm detects and what actions the recipients should take\.

1. In the **Instance** section, you can enable or disable all alarm instances that are created based on this alarm model\.

1. In the **Permissions and alarm state actions** section, do the following:

   1. For **IAM role**, choose one of the following options:
      + Choose an existing IAM role that allows this alarm model to perform actions and access other AWS resources\.
      + Enter a role name\. AWS IoT Events automatically creates an IAM role with the name and grants the role the required permissions\.

      For more information, see [Identity and access management for AWS IoT Events](https://docs.aws.amazon.com/iotevents/latest/developerguide/security-iam.html)\.

   1. In **Advanced flow**, you can enable or disable **Acknowledge flow**\.
      + If you choose **Enabled**, you receive a notification when the alarm state changes\. You must acknowledge the notification before the alarm state can return to normal\.
      + If you choose **Disabled**, no action is required\. The alarm automatically changes to the normal state when the measurement returns to the specified range\.

      For more information, see [Acknowledge flow](iotevents-alarms.md#acknowledge-flow)\.

1. \(Optional\) You can add **Tags** to manage your alarms\. For more information, see [Tagging your AWS IoT Events resources](https://docs.aws.amazon.com/iotevents/latest/developerguide/tagging-iotevents.html)\.

1. Choose **Create**