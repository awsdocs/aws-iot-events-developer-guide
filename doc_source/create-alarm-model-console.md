# Creating an alarm model \(console\)<a name="create-alarm-model-console"></a>

The following shows you how to create an alarm model to monitor an AWS IoT Events attribute in the AWS IoT Events console\.

1. Sign in to the [AWS IoT Events console](https://console.aws.amazon.com/iotevents/)\.

1. In the navigation pane, choose **Alarm models**\.

1. On the **Alarm models** page, choose **Create alarm model**\.

1. In the **Alarm model details** section, do the following:

   1. Enter a unique name\.

   1. \(Optional\) Enter a description\.

1. In the **Alarm target** section, do the following:
**Important**  
If you choose **AWS IoT SiteWise asset property**, you must have created an asset model in AWS IoT SiteWise\.

   1. Choose **AWS IoT Events input attribute**\.

   1. Choose the input\.

   1. Choose the input attribute key\. This input attribute is used as a key to create the alarm\. AWS IoT Events routes inputs associated with this key to the alarm\.

1. In the **Threshold definitions** section, you define the input attribute, threshold value, and comparison operator that AWS IoT Events uses to change the state of the alarm\.

   1. For **Input attribute**, choose the attribute that you want to monitor\.

      Each time that this input attribute receives new data, it's evaluated to determine the state of the alarm\.

   1. For **Operator**, choose the comparison operator\. The operator compares your input attribute with the threshold value for your attribute\.

      You can choose from these options: 
      + **> greater than**
      + **>= greater than or equal to**
      + **< less than**
      + **<= less than or equal to**
      + **= equal to**
      + **\!= not equal to**

   1. For threshold **Value**, enter a number or choose an attribute in AWS IoT Events inputs\. AWS IoT Events compares this value with the value of the input attribute you choose\. 

   1. \(Optional\) For **Severity**, Use a number that your team understands to reflect the severity of this alarm\.

1. \(Optional\) In the **Notification settings** section, configure notification settings for the alarm\.

   You can add up to 10 notifications\. For **Notification 1**, do the following:

   1. For **Protocol**, choose from the following options:
      + **Email & text** \- The alarm sends an SMS notification and an email notification\.
      + **Email** \- The alarm sends an email notification\.
      + **Text** \- The alarm sends an SMS notification\.

   1. For **Sender**, specify the email address that can send notifications about this alarm\.

      To add more email addresses to your sender list, choose **Add sender**\. 

   1. \(Optional\) For **Recipient**, choose the recipient\.

      To add more users to your recipient list, choose **Add new user**\. You must add new users to your IAM Identity Center store before you can add them to your alarm model\. For more information, see [Managing recipients](sso-authorization-recipients.md)\.

   1. \(Optional\) For **Additional custom message**, enter a message that describes what the alarm detects and what actions the recipients should take\.

1. In the **Instance** section, you can enable or disable all alarm instances that are created based on this alarm model\.

1. In the **Advanced settings** section, do the following:

   1. For **Acknowledge flow**, you can enable or disable notifications\.
      + If you choose **Enabled**, you receive a notification when the alarm state changes\. You must acknowledge the notification before the alarm state can return to normal\.
      + If you choose **Disabled**, no action is required\. The alarm automatically changes to the normal state when the measurement returns to the specified range\.

      For more information, see [Acknowledge flow](iotevents-alarms.md#acknowledge-flow)\.

   1. For **Permissions**, choose one of the following options:
      + You can **Create a new role from AWS policy templates** and AWS IoT Events automatically creates an IAM role for you\. 
      + You can **Use an existing IAM role** that allows this alarm model to perform actions and access other AWS resources\.

      For more information, see [Identity and access management for AWS IoT Events](https://docs.aws.amazon.com/iotevents/latest/developerguide/security-iam.html)\.

   1. For **Additional notification settings**, you can edit your AWS Lambda function to manage alarm notifications\. Choose one of the following options for your AWS Lambda function:
      + **Create a new AWS Lambda function** \- AWS IoT Events creates a new AWS Lambda function for you\.
      + **Use an existing AWS Lambda function** \- Use an existing AWS Lambda function by choosing an AWS Lambda function name\. 

      For more information about the possible actions, see [Working with other AWS services](iotevents-other-aws-services.md)\.

   1. \(Optional\) For **Set state action**, you can add one or more AWS IoT Events actions to take when the alarm state changes\.

1. \(Optional\) You can add **Tags** to manage your alarms\. For more information, see [Tagging your AWS IoT Events resources](https://docs.aws.amazon.com/iotevents/latest/developerguide/tagging-iotevents.html)\.

1. Choose **Create**\.