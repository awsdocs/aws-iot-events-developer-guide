# Cross\-service confused deputy prevention<a name="cross-service-confused-deputy-prevention"></a>

**Note**  
The AWS IoT Events service only allows customers to use roles to start actions in the same account that a resource was created\. This means that a confused deputy attack cannot be performed with this service\.
This page serves as a reference for customers to see how the confused deputy issue works and can be prevented in the case that cross account resources were allowed in the AWS IoT Events service\.

The confused deputy problem is a security issue where an entity that doesn't have permission to perform an action can coerce a more\-privileged entity to perform the action\. In AWS, cross\-service impersonation can result in the confused deputy problem\. Cross\-service impersonation can occur when one service \(the *calling service*\) calls another service \(the *called service*\)\. The calling service can be manipulated to use its permissions to act on another customer's resources in a way it should not otherwise have permission to access\. To prevent this, AWS provides tools that help you protect your data for all services with service principals that have been given access to resources in your account\. 

We recommend using the [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn) and [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount) global condition context keys in resource policies to limit the permissions that AWS IoT Events gives another service to the resource\. If the `aws:SourceArn` value does not contain the account ID, such as an Amazon S3 bucket ARN, you must use both global condition context keys to limit permissions\. If you use both global condition context keys and the `aws:SourceArn` value contains the account ID, the `aws:SourceAccount` value and the account in the `aws:SourceArn` value must use the same account ID when used in the same policy statement\. 

 Use `aws:SourceArn` if you want only one resource to be associated with the cross\-service access\. Use `aws:SourceAccount` if you want to allow any resource in that account to be associated with the cross\-service use\.  The value of `aws:SourceArn` must be the Detector Model or Alarm model associated with the `sts:AssumeRole` request\.

The most effective way to protect against the confused deputy problem is to use the `aws:SourceArn` global condition context key with the full ARN of the resource\. If you don't know the full ARN of the resource or if you are specifying multiple resources, use the `aws:SourceArn` global context condition key with wildcards \(`*`\) for the unknown portions of the ARN\. For example, `arn:aws:iotevents:*:123456789012:*`\. 

The following examples show how you can use the `aws:SourceArn` and `aws:SourceAccount` global condition context keys in AWS IoT Events to prevent the confused deputy problem\.

**Topics**
+ [Example 1: Accessing a Detector Model](#accessing-a-detector-model)
+ [Example 2: Accessing an Alarm Model](#accessing-an-alarm-model)
+ [Example 3: Accessing a Resource in a Specified Region](#accessing-resource-in-specified-region)
+ [Example 4: Logging Options](#logging-options)

## Example 1: Accessing a Detector Model<a name="accessing-a-detector-model"></a>

The following role can only be used to access a DetectorModel named `foo`\.

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
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "account_id"
        },
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:iotevents:region:account_id:detectorModel/foo"
        }
      }
    }
  ]
 }
}
```

## Example 2: Accessing an Alarm Model<a name="accessing-an-alarm-model"></a>

The following role can only be used to access any Alarm Model\.

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
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "account_id"
        },
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:iotevents:region:account_id:alarmModel/*"
        }
      }
    }
  ]
}
```

## Example 3: Accessing a Resource in a Specified Region<a name="accessing-resource-in-specified-region"></a>

The following example shows a role that you can use to access a resource in a specified region\. The region in this example is *us\-east\-1*\.

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
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "account_id"
        },
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:iotevents:us-east-1:account_id:*"
        }
      }
    }
  ]
}
```

## Example 4: Logging Options<a name="logging-options"></a>

To provide a role for logging options, you will need to allow it to be assumed for every resource in IoT Events\. Accordingly, you'll have to use a wildcard \(\*\) for the resource type and the resource name\.

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
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "account_id"
        },
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:iotevents:region:account_id:*"
        }
      }
    }
  ]
}
```