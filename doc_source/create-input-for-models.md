# Create an input in the Navigation Pane<a name="create-input-for-models"></a>

This topic shows how to create an *input*, for an alarm model or a detector model, through the navigation pane\.

1. Log into the [AWS IoT Events console ](https://console.aws.amazon.com/iotevents/) or select the option to Create a new AWS IoT Events account\.

1. In the AWS IoT Events console, in the upper left corner, select and expand the navigation pane\.

1. In the left navigation pane, select **Inputs**\.

1. In the right corner of the console, choose **Create input**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-input-navigation.png)

1. For the input, enter an **InputName**, an optional **Description**, and choose **Upload file**\. In the dialog box that displays, select the `input.json` file that you created in the overview for [create an input](https://docs.aws.amazon.com/iotevents/latest/developerguide/create-input-overview.html)\.   
![\[Create an input in the AWS IoT Events console.\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-input-name.png)

1. For **Choose input attributes**, select the attributes to use, and choose **Create**\. In this example, we select **motorid** and **sensorData\.pressure**\.   
![\[Create an input in the AWS IoT Events console.\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-input-attr.png)