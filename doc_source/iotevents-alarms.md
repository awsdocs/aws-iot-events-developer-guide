# Monitoring with alarms<a name="iotevents-alarms"></a>


|  | 
| --- |
|  The alarms feature is in preview release for AWS IoT Events, AWS IoT SiteWise, and SiteWise Monitor, and is subject to change\. We recommend that you use this feature only with test data, and not in production environments\. While the alarms feature is in preview, you must download the alarms preview AWS SDK to use the API operations for this feature\. These API operations aren't available in the public AWS SDK\. For more information, see [Alarms preview AWS CLI and AWS SDKs](https://docs.aws.amazon.com/iotevents/latest/developerguide/alarms-preview-SDKs.html)\.  | 

AWS IoT Events alarms help you monitor your data for changes\. The data can be metrics that you measure for your equipment and processes\. You can create alarms that send notifications when a threshold is breached\. Alarms help you detect issues, streamline maintenance, and optimize performance of your equipment and processes\.

Like detectors, alarms are instances of alarm models\. The alarm model specifies what to detect, when to send notifications, who gets notified, and more\. You can also specify one or more [supported actions](https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-supported-actions.html) that occur when the alarm state changes\. AWS IoT Events routes [input attributes](https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-detector-input.html) derived from your data to the appropriate alarms\. If the data that you're monitoring is outside the specified range, the alarm is invoked\. You can also acknowledge the alarms or set them to the snooze mode\.

## Working with AWS IoT SiteWise<a name="alarms-collaborations.title"></a>

You can use AWS IoT Events alarms to monitor asset properties in AWS IoT SiteWise\. AWS IoT SiteWise sends asset property values to AWS IoT Events alarms\. AWS IoT Events sends the alarm state to AWS IoT SiteWise\.

AWS IoT SiteWise also supports external alarms\. You might choose external alarms if you use alarms outside of AWS IoT SiteWise and have a solution that returns alarm state data\. The external alarm contains a measurement property that ingests the alarm state data\.

AWS IoT SiteWise doesn't evaluate the state of external alarms\. Additionally, you can't acknowledge or snooze an external alarm when the alarm state changes\.

You can use the SiteWise Monitor feature to view the state of external alarms in SiteWise Monitor portals\.

For more information, see [Monitoring data with alarms](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/industrial-alarms.html) in the *AWS IoT SiteWise User Guide* and [Monitoring with alarms](https://docs.aws.amazon.com/iot-sitewise/latest/appguide/monitor-alarms.html) in the *AWS IoT SiteWise Monitor Application Guide*\.

## Acknowledge flow<a name="acknowledge-flow"></a>

When you create an alarm model, you choose whether to enable acknowledge flow\. If you enable acknowledge flow, your team gets notified when the alarm state changes\. Your team can acknowledge the alarm and leave a note\. For example, you can include the information of the alarm and the actions that you're going to take to address the issue\. If the data that you're monitoring is outside the specified range, the alarm is invoked\.

Alarms have the following states:

`DISABLED`  
When the alarm is in the `DISABLED` state, it isn't ready to evaluate data\. To enable the alarm, you must change the alarm to the `NORMAL` state\.

`NORMAL`  
When the alarm is in the `NORMAL` state, it's ready to evaluate data\.

`ACTIVE`  
If the alarm is in the `ACTIVE` state, the alarm is invoked\. The data that you're monitoring is outside the specified range\.

`ACKNOWLEDGED`  
When the alarm is in the `ACKNOWLEDGED` state, the alarm was invoked and you acknowledged the alarm\.

`LATCHED`  
The alarm was invoked, but you didn't acknowledge the alarm after a period of time\. The alarm automatically changes to the `NORMAL` state\.

`SNOOZE_DISABLED`  
When the alarm is in the `SNOOZE_DISABLED` state, the alarm is disabled for a specified period of time\. After the snooze time, the alarm automatically changes to the `NORMAL` state\.