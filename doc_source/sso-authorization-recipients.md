# Managing recipients<a name="sso-authorization-recipients"></a>

AWS IoT Events uses AWS IAM Identity Center \(successor to AWS Single Sign\-On\) \(IAM Identity Center\) to manage the SSO access of alarms recipients\. To enable the alarm to send notifications to the recipients, you must enable IAM Identity Center and add recipients to your IAM Identity Center store\. For more information, see [Add Users](https://docs.aws.amazon.com/singlesignon/latest/userguide/addusers.html) in *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.

**Important**  
You must choose the same AWS Region for AWS IoT Events, AWS Lambda, and IAM Identity Center\.
AWS Organizations only supports one IAM Identity Center Region at a time\. If you want to make IAM Identity Center available in a different Region, you must first delete your current IAM Identity Center configuration\. For more information, see [IAM Identity Center Region Data](https://docs.aws.amazon.com/singlesignon/latest/userguide/regions.html#region-data) in *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.