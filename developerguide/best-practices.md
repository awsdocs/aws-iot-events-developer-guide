# Best Practices for AWS IoT Events<a name="best-practices"></a>

Use these best practices to help you get the maximum benefit from AWS IoT Events\.

**Topics**
+ [Enable Amazon CloudWatch Logging](#best-practices-cw-logs)
+ [Working in the AWS IoT Events console](#best-practices-console)
+ [Avoid Possible Data Loss Due to Inactivity](#best-practices-inactivity)

## Enable Amazon CloudWatch Logging<a name="best-practices-cw-logs"></a>

When you develop or debug an AWS IoT Events detector model, you need to know what AWS IoT Events is doing, and any errors it encounters\. Amazon CloudWatch monitors your Amazon Web Services \(AWS\) resources and the applications you run on AWS in real time\. With CloudWatch, you gain systemwide visibility into resource use, application performance, and operational health\. 

1. If you haven't already, follow the steps in [Setting Up Permissions for AWS IoT Events](iotevents-start.md#iotevents-permissions) to create a role with an attached policy that grants permission to create and manage CloudWatch logs for AWS IoT Events\.

1. In the AWS IoT Events console, choose the menu icon in the upper\-left corner to open the navigation pane\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/start.png)

   If you are on the **Getting started** page, choose the **X** in the upper right close that page and go to the **Detector model palette**\. Then you can choose the menu icon in the upper\-left corner\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/welcomeX.png)

1. In the navigation pane, choose **Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/left-nav.png)

1. On the **Settings** page, choose **Edit**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/settings.png)

1. On the **Edit logging options** page, choose the **Level of verbosity**\. For **Select role**, select a role with sufficient permissions to perform the logging actions you chose\. If you choose **Debug** for the level of verbosity, you can also choose **Add Model Option** then add a **Detector Model Name** and optional **KeyValue** to specify the detector model\(s\) and specific detectors \(instances\) to log\. Then choose **Update**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/edit-logging-options.png)

1. Your logging options are successfully updated\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/logging-options-updated.png)

## Working in the AWS IoT Events console<a name="best-practices-console"></a>

When you use the AWS IoT Events console, your work in progress is saved locally in your browser \(we use cookies\)\. But you must choose **Publish** to save your detector model to AWS IoT Events\. After you publish a detector model, your published work is available in any browser you use to access your account\. Before you publish, your work isn't saved\. 

**Note**  
After you publish a detector model, you can't change its name\. You can, however, continue to modify its definition\.

## Avoid Possible Data Loss Due to Inactivity<a name="best-practices-inactivity"></a>

If you do not use AWS IoT Events for a significant period of time \(that is, you incur no charges and create no new detector models\) then your data, including your detector models, may be deleted automatically\. However, we will not delete any data or detector models without providing you with at least 30 days prior notice\. If you need to store any data for an extended period of time, consider using an AWS storage specific service as described in [ AWS Storage Services](https://docs.aws.amazon.com/whitepapers/latest/cost-optimization-storage-optimization/aws-storage-services.html)\.