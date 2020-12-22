# Responding to alarms<a name="respond-to-alarms"></a>

If you enabled [acknowledge flow](https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-alarms.html#acknowledge-flow), you receive notifications when the alarm state changes\. To respond to the alarm, you can acknowledge, disable, enable, reset, or snooze the alarm\.

## Responding to alarms \(console\)<a name="respond-to-alarms-console"></a>

The following shows you how to respond to an alarm in the AWS IoT Events console\.

1. Sign in to the [AWS IoT Events console](https://console.aws.amazon.com/iotevents/)\.

1. In the navigation pane, choose **Alarm models**\.

1. Choose the target alarm model\.

1. In the **List of alarms** section, choose the target alarm\.

1. You can choose one of the following options from **Actions**:
   + **Acknowledge** \- The alarm changes to the `ACKNOWLEDGED` state\.
   + **Disable** \- The alarm changes to the `DISABLED` state\.
   + **Enable** \- The alarm changes to the `NORMAL` state\.
   + **Reset** \- The alarm changes to the `NORMAL` state\.
   + **Snooze**, and then do the following:

     1. Choose the **Snooze length** or enter a **Custom snooze length**\.

     1. Choose **Save**\.

     The alarm changes to the `SNOOZE_DISABLED` state

   For more information about the alarm states, see [Acknowledge flow](iotevents-alarms.md#acknowledge-flow)\.

## Responding to alarms \(API\)<a name="respond-alarms-API"></a>

To respond to one or more alarms, you can use the following AWS IoT Events APIs:
+ [BatchAcknowledgeAlarm](https://docs.aws.amazon.com/iotevents/latest/apireference/API_iotevents-data_BatchAcknowledgeAlarm.html)
+ [BatchDisableAlarm](https://docs.aws.amazon.com/iotevents/latest/apireference/API_iotevents-data_BatchDisableAlarm.html)
+ [BatchEnableAlarm](https://docs.aws.amazon.com/iotevents/latest/apireference/API_iotevents-data_BatchEnableAlarm.html)
+ [BatchResetAlarm](https://docs.aws.amazon.com/iotevents/latest/apireference/API_iotevents-data_BatchResetAlarm.html)
+ [BatchSnoozeAlarm](https://docs.aws.amazon.com/iotevents/latest/apireference/API_iotevents-data_BatchSnoozeAlarm.html)