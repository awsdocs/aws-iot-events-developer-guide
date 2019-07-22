# Setting Up AWS IoT Events<a name="iotevents-start"></a>

If you don't have an AWS account, create one\. 

**To create an AWS account**

1. Open the [AWS home page](https://aws.amazon.com/), and then choose **Create an AWS Account**\.

1. Follow the online instructions\. Part of the sign\-up procedure involves receiving a phone call and entering a PIN using your phone's keypad\.

**Topics**
+ [Setting Up Permissions for AWS IoT Events](#iotevents-permissions)

## Setting Up Permissions for AWS IoT Events<a name="iotevents-permissions"></a>

This section describes the roles and permissions that are required to use some features of AWS IoT Events\. You can use AWS CLI commands or the AWS Identity and Access Management \(IAM\) console to create roles and associated permission policies to access resources or perform certain functions in AWS IoT Events\. 

The [ AWS Identity and Access Management User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) has more detailed information about securely controlling permissions to access AWS resources\. For information specific to AWS IoT Events, see [Actions, Resources, and Condition Keys for AWS IoT Events](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsiotevents.html)\.

### Action Permissions<a name="iotevents-permissions-event-actions"></a>

AWS IoT Events allows you to trigger actions which make use of other AWS services\. To do so, you must grant AWS IoT Events permission to perform these actions on your behalf\. This section contains a list of the actions and an example policy which grants permission to perform all these actions on your resources\. \(You should change the "region" and "account\-id" references as required\. When possible, you should also change the wildcards \(\*\) to refer to specific resources that will be accessed\.\) [ Using the IAM Console to Manage Roles and Permissions](#iotevents-permissions-console) shows how to use the IAM console to grant permission to AWS IoT Events to send an Amazon SNS alert you have defined\.

AWS IoT Events provides these types of actions for an event:
+ Send a message using the [ Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) \(`actions.sns`\)\.
+ Send a message using the [ AWS IoT Message Broker](https://docs.aws.amazon.com/iot/latest/developerguide/iot-message-broker.html) \(`actions.iotTopicPublish`\)\.
+ Set the value of a variable \(`actions.setVariable`\)\. \(No additional permissions are required for this type of action\.\)
+ Set, reset, or clear a timer \(`actions.setTimer|resetTimer|clearTimer`\)\. \(No additional permissions are required for this type of action\.\)
+ Call a [Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) function, passing in information about the detector model instance and the event which triggered the action \(`actions.lambda`\)\.
+ Send an input back in to AWS IoT Events, passing information about the detector model instance and the event which triggered the action \(`actions.iotEvents`\)\.
+ Send detector model instance and triggering event information to an [ Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) queue \(`actions.sqs`\)\.
+ Send detector model instance and triggering event information to a [Kinesis Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html) stream \(`actions.firehose`\)\.

Example policy:

```
{
   "Version": "2012-10-17",
   "Statement": [
     {
       "Effect": "Allow",
       "Action": "iot:Publish",
       "Resource": "arn:aws:iot:region:account_id:topic/*"
     },
     {
       "Effect": "Allow",
       "Action": "sns:Publish",
       "Resource": "arn:aws:sns:region:account_id:*"
     },
     {
       "Effect": "Allow",
       "Action": "sqs:SendMessage",
       "Resource": "arn:aws:sqs:region:account_id:*"
     },
     {
       "Effect": "Allow",
       "Action": "lambda:InvokeFunction",
       "Resource": "arn:aws:lambda:region:account_id:function:*"
     },
     {
       "Effect": "Allow",
       "Action": [
         "firehose:PutRecord",
         "firehose:PutRecordBatch"
       ],
       "Resource": "arn:aws:firehose:region:account_id:deliverystream/*"
     },
     {
       "Effect": "Allow",
       "Action": "iotevents:BatchPutMessage",
       "Resource": "arn:aws:iotevents:region:account_id:input/*"
     }
   ]
}
```

### Securing Input Data<a name="iotevents-permissions-input-data"></a>

It's important to consider who can grant access to input data for use in a detector model\. If you have a user or entity whose overall permissions you want to restrict, but that is permitted to create or update a detector model, you must also grant permission for that user or entity to update input routing\. This means that in addition to granting permission for "`iotevents:CreateDetectorModel`" and "`iotevents:UpdateDetectorModel`", you must also grant permission for "`iotevents:UpdateInputRouting`"\.

The following is an example policy that adds this permission\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "updateRoutingPolicy",
            "Effect": "Allow",
            "Action": [
                "iotevents:UpdateInputRouting"
            ],
            "Resource": "*"
        }
    ]
}
```

You can specify a list of input Amazon Resource Names \(ARNs\) instead of the wildcard "`*`" for the "`Resource`" to limit this permission to specific inputs\. This gives you the ability to restrict access to the input data that is consumed by detector models created or updated by the user or entity\.

### Using the IAM Console to Manage Roles and Permissions<a name="iotevents-permissions-console"></a>

The following example shows how to use the IAM console to grant permission to AWS IoT Events to generate Amazon SNS alerts on your behalf\. You can also attach the role to any entity \(user or account owner\) that you will use to define and publish an AWS IoT Events detector model that contains event actions that send Amazon SNS alerts\.

To complete this example, you need the ARN of an existing [Amazon SNS topic](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) to use in AWS IoT Events\.

1. Sign in to the AWS Management Console, and then open the [IAM console](https://console.aws.amazon.com/iam/home)\.

1. In the navigation pane, choose **Dashboard**, **Roles**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/welcome-iam.png)

1. Choose **Create role**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-role.png)

1. On the **Create role** page, for **Select type of trusted entity**, choose **AWS service**\. For **Choose the service that will use this role**, choose **IoT**\. For **Select your use case**, choose **IoT**\. Then choose **Next: Permissions**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/choose-service-1.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/choose-service-2.png)

1. On the second part of the **Create role** page, for **Attached permissions**, leave these permissions as they are for now\. You add a new policy granting the required permissions in a later step\. Choose **Next: Tags**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/next-tags.png)

1. The third part of the **Create role** page, for **Add tags \(optional\)**, don't add any tags right now\. Choose **Next:Review**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/next-review.png)

1. On the **Review** page, enter a **Role name**\. You can also enter a **Role description**\. Then choose **Create role**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-role-final.png)

1. On the **Roles** page, find and select the name of the role you just created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/select-role.png)

1. On the **Summary** page for your role, on the **Permissions** tab, choose **Attach policies**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/attach-policies.png)

1. On the **Attach Permissions** page, choose **Create policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-policy.png)

1. On the **Create Policy** page, select the **JSON** tab\. If a policy validation failure warning appears, you can safely dismiss it by choosing the **X** in the message\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-policy-json.png)

1. Replace the JSON in the editor with the following\. and change the "`Resource`" value to the ARN of the [SNS topic](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) you want to use in AWS IoT Events\. Then choose **Review policy**\. \(You can also use wildcard characters in the SNS topic ARN to grant broader permissions, but be aware of the security issues this raises\.\)

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Action": [
                   "sns:*"
               ],
               "Effect": "Allow",
               "Resource": "arn:aws:sns:us-east-1:123456789012:testAction"
           }
       ]
   }
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/review-policy.png)

1. On the **Review policy** page, enter a **Name** for the policy\. You can also enter a **Description**\. Then choose **Create policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/review-policy-name.png)

1. On the **Policies** page, in the navigation pane, choose **Roles**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/from-policy-to-role.png)

1. On the **Roles** page, find and select the name of the role you created earlier\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/select-role-again.png)

1. On the **Summary** page for your role, on the **Permissions** tab, choose **Attach policies**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/attach-policies.png)

1. On the **Attach permissions** page for your role, find the policy you created, select the box next to it, and then choose **Attach policy**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/add-permissions.png)

1. Again on the **Roles** page, find and select the name of the role you created earlier\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/select-role-again.png)

1. On the **Summary** page for that role, on the **Trust relationships** tab, choose **Edit trust relationship**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/edit-trust.png)

1. On the **Edit Trust Relationship** page, for **Policy Document**, replace the existing JSON with the following\. Then choose **Update Trust Policy**\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": "iotevents.amazonaws.com" 
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/update-trust.png)

You have now granted AWS IoT Events permission to send alerts to your SNS topic on your behalf\. For enhanced security, remove the unused policies that were attached by default to the role you created\.

### CloudWatch Logging Role Policy<a name="iotevents-permissions-cloudwatch"></a>

The following policy documents provide the role policy and trust policy that allow AWS IoT Events to submit logs to Amazon CloudWatch on your behalf\.

Role policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:PutMetricFilter",
                "logs:PutRetentionPolicy",
                "logs:GetLogEvents",
                "logs:DeleteLogStream"
            ],
            "Resource": [
                "arn:aws:logs:*:*:*"
            ]
        }
    ]
}
```

Trust policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
          
          "iotevents.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

You also need an IAM permissions policy attached to the IAM user that allows the user to pass roles, as follows\.For more information, see [Granting a User Permissions to Pass a Role to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Action": [
          "iam:GetRole",
          "iam:PassRole"
      ],
      "Resource": "arn:aws:iam::<account-id>:role/Role_To_Pass"
    }
  ]
}
```

You can use the following command to put the resource policy for CloudWatch logs\. This allows AWS IoT Events to put log events into CloudWatch streams\.

```
aws logs put-resource-policy --policy-name ioteventsLoggingPolicy --policy-document "{ \"Version\": \"2012-10-17\", \"Statement\": [ { \"Sid\": \"IoTEventsToCloudWatchLogs\", \"Effect\": \"Allow\", \"Principal\": { \"Service\": [ \"iotevents.amazonaws.com\" ] }, \"Action\":\"logs:PutLogEvents\", \"Resource\": \"*\" } ] }" 
```

Use the following command to put logging options, substituting the `roleArn` with the logging role you created\.

```
aws iotevents put-logging-options --cli-input-json "{ \"loggingOptions\": {\"roleArn\": \"arn:aws:iam::123456789012:role/testLoggingRole\", \"level\": \"INFO\", \"enabled\": true } }" 
```

### Amazon SNS Messaging Role Policy<a name="iotevents-permissions-sns"></a>

The following policy documents provide the role policy and trust policy that allow AWS IoT Events to send SNS messages\.

Role policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "sns:*"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:sns:us-east-1:123456789012:testAction"
        }
    ]
}
```

Trust policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "iotevents.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```