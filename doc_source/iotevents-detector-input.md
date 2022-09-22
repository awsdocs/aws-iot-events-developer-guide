# Create an input in the Detector Model<a name="iotevents-detector-input"></a>

This topic shows how to define an *input* for a detector model to receive telemetry data, or messages\.

1. Open the [AWS IoT Events console](https://console.aws.amazon.com/iotevents/)\.

1. In the AWS IoT Events console, choose **Create detector model**\.  
![\[Create a detector model in the AWS IoT Events console.\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/start.png)

1. Choose **Create new**\.  
![\[Create a detector model in the AWS IoT Events console.\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/welcome.png)

1. Choose **Create input**\.  
![\[Create an input in the AWS IoT Events console.\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-input.png)

1. For the input, enter an **InputName**, an optional **Description**, and choose **Upload file**\. In the dialog box that displays, select the `input.json` file that you created in the overview for [create an input](https://docs.aws.amazon.com/iotevents/latest/developerguide/create-input-overview.html)\.   
![\[Create an input in the AWS IoT Events console.\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-input-name.png)

1. For **Choose input attributes**, select the attributes to use, and choose **Create**\. In this example, we select **motorid** and **sensorData\.pressure**\.   
![\[Create an input in the AWS IoT Events console.\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-input-attr.png)