# Troubleshooting a detector model by running analyses<a name="iotevents-analyze-api"></a>

AWS IoT Events can analyze your detector model and generate analysis results without sending input data to your detector model\. AWS IoT Events performs a series of analyses described in this section to check your detector model\. This advanced troubleshooting solution also summarizes diagnostic information, including the severity level and location, so that you can quickly find and fix potential issues in your detector model\. For more information about diagnostic error types and messages for your detector model, see [Detector model analysis and diagnostic information](analyze-diagnostic-information.md)\.

You can use the AWS IoT Events console, [API](https://docs.aws.amazon.com/iotevents/latest/apireference/), [AWS Command Line Interface \(AWS CLI\)](https://docs.aws.amazon.com/cli/latest/reference/iotevents/index.html), or [AWS SDK](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sdks.html) to view diagnostic error messages from the analysis of your detector model\.

**Note**  
You must fix all errors before you can publish your detector model\.
We recommend that you review warnings and take necessary actions before you use your detector model in production environments\. Otherwise, the detector model might not work as expected\.
You can have up to 10 analyses in the `RUNNING` status at the same time\.

To learn how to analyze your detector model, see [Analyzing a detector model \(Console\)](analyze-api-console.md) or [Analyzing a detector model \(AWS CLI\)](analyze-api-api.md)\. 

**Topics**
+ [Detector model analysis and diagnostic information](analyze-diagnostic-information.md)
+ [Analyzing a detector model \(Console\)](analyze-api-console.md)
+ [Analyzing a detector model \(AWS CLI\)](analyze-api-api.md)