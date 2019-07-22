# Create an Input<a name="iotevents-detector-input"></a>

In this topic, you define an *input* to receive telemetry data \(messages\)\.

1. Open the AWS IoT Events console: [https://console\.aws\.amazon\.com/iotevents/](https://console.aws.amazon.com/iotevents/)\.

1. In the AWS IoT Events console, choose **Create detector model**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/start.png)

1. Choose **Create new**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/welcome.png)

1. When you construct your own detector models and inputs, you might want to gather files containing example message payloads that your devices or processes send to report their status\. This makes it easier for you to define the inputs you need\. 

   For this example, on your local file system, create a file named "`input.json`" with the following contents\.

   ```
   {
     "motorid": "Fulton-A32",
     "sensorData": {
       "pressure": 23,
       "temperature": 47
     }
   }
   ```

1. Choose **Create input**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-input.png)

1. For the input, provide an **InputName** and a **Description**\. Select **Upload file**, and in the dialog box that appears, choose the JSON file that contains the example message \("input\.json"\)\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-input-name.png)

1. Select the boxes next to attributes that you want to use \(for this example, **motorid** and **sensorData\.pressure**\)\. Clear the box next to any message attribute you don't want to use \(this example doesn't use **sensorData\.temperature**\)\. Then choose **Create**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-input-attr.png)