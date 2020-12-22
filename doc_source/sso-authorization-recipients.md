# Managing recipients<a name="sso-authorization-recipients"></a>

AWS IoT Events uses AWS Single Sign\-On \(AWS SSO\) to manage the SSO access of alarms recipients\. To enable the alarm to send notifications to the recipients, you must enable AWS SSO and add recipients to your AWS SSO store\. For more information, see [Add Users](https://docs.aws.amazon.com/singlesignon/latest/userguide/addusers.html) in *AWS Single Sign\-On User Guide*\.

**Important**  
You must choose the same AWS Region for AWS IoT Events, AWS Lambda, and AWS SSO\.
AWS Organizations only supports one AWS SSO Region at a time\. If you want to make AWS SSO available in a different Region, you must first delete your current AWS SSO configuration\. For more information, see [AWS SSO Region Data](https://docs.aws.amazon.com/singlesignon/latest/userguide/regions.html#region-data) in *AWS Single Sign\-On User Guide*\.