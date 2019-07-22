# Create a Detector Model<a name="iotevents-detector-model"></a>

In this topic, you define a *detector model* \(a model of your equipment or process\) using *states*\.

For each state, you define conditional \(Boolean\) logic that evaluates the incoming inputs to detect a significant event\. When an event is detected, it changes the state and can trigger additional actions\. These events are known as "transition" events\.

In your states, you'll also define events that can execute actions whenever the detector enters or exits that state, or when an input is received \(these are known as `OnEnter`, `OnExit` and `OnInput` events\)\. The actions are executed only if the event's conditional logic evaluates to `true`\.

1. The first detector state has been created for you\. To modify it, click in the circle with label **State\_1** in the main editing space\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-state-init.png)

1. In the **State** pane, enter the **State name**\. Then, for **OnEnter**, choose **Add event**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/add-event-on-enter.png)

1. On the **Add OnEnter event** page, enter an **Event name** and the **Event condition** \(in this example, just `true` to indicate the event is always triggered when the state is entered\)\. Under **Event actions**, choose **Set variable**, and then enter the name of the variable to set\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/add-onenter-1.png)

1. Scroll to **Select operation**, and then choose **Assign value**\. Under **Variable value**, enter the value **0** \(zero\)\. Then, choose **Save**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/add-onenter-2.png)

   A variable, like the one you just defined, can be set \(given a value\) in any event in the detector model\. But its value can be referenced \(for example, in an event's conditional logic\) only after the detector has reached a state and executed an action where it is defined or set\.

1. In the **State** pane, choose the **X** next to **State** to return to the **Detector model palette**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/pre-transition-event.png)

1. To create a second detector state, in the **Detector model palette**, choose **State**\. Drag it into the main editing space\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/create-state-2.png)

1. Place your cursor over the first state \(**Normal**\)\. An arrow appears on the circumference of the state\. Click and drag the arrow from the first state to the second state\. A directed line from the first state to the second state \(labeled **Untitled**\) appears\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/transition-untitled.png)

1. Select the **Untitled** line\. In the **Transition event** pane, enter an **Event name** and **Event trigger logic**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/transition-event-1.png)

1. Choose **Add action**\. On the **Add transition event actions** page, for **Add action**, choose **Set variable**\. Enter a **Variable name**\. For **Select operation**, choose **Assign value**\. Enter a **Variable value**, and then choose **Save**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/transition-event-2.png)

1. Select the second state\. In the **State** pane, enter the **State name**\. For **On Enter**, choose **Add event**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/second-state-onenter.png)

1. On the **Add OnEnter event** page, enter the **Event name** and **Event condition**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/sns-action-1.png)

1. For **Add action**, choose **Send SNS message**\. Enter the **Target arn** of your SNS topic, and then choose **Save**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/sns-action-2.png)

1. Continue to add the events in the example\.

   For **OnInput**, choose **Add event**, and then enter and save the following event information\.

   ```
     Event name: Overpressurized
     Event condition: $input.PressureInput.sensorData.pressure > 70
     Event actions:
       Set variable:
         Variable name: pressureThresholdBreached
         Select operation: Assign value
         Variable value: 3
   ```

   For **OnInput**, choose **Add event**, and then enter and save the following event information\.

   ```
     Event name: Pressure Okay
     Event condition: $input.PressureInput.sensorData.pressure <= 70
     Event actions:
       Set variable:
         Variable name: pressureThresholdBreached
         Select operation: Decrement
   ```

   For **OnExit**, choose **Add event**, and then enter and save the following event information \(using the ARN of the SNS topic you created\)\.

   ```
     Event name: Normal Pressure Restored
     Event condition: true
     Event actions:
       Send SNS message: 
         Target arn: arn:aws:sns:us-east-1:123456789012:pressureClearedAction
   ```

1. Place your cursor over the second state \(`Dangerous`\)\. An arrow appears on the circumference of the state\. Click and drag the arrow from the second state to the first state\. A directed line with label **Untitled** appears\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/back-to-normal.png)

1. Choose the **Untitled** line\. In the **Transition event** pane, enter an **Event name** and **Event trigger logic** using the following information\.

   ```
   {
     Event name: BackToNormal
     Event trigger logic: $input.PressureInput.sensorData.pressure <= 70 && $variable.pressureThresholdBreached <= 0
   }
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/back-to-normal-event.png)

   For an explanation of why we test for both the `$input` value and the `$variable` value in the trigger logic here, see the entry under "Availability of variable values" in [Detector Model Restrictions and Limitations](iotevents-restrictions-detector-model.md)\.

1. Select the **Start** state \(it was created by default when you first began creating a detector model\)\. In the **Start** pane, choose the **Destination state** \(**Normal** in this example\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/start-normal.png)

1. In the upper\-right corner, choose **Publish**\. On the **Edit detector model** page, enter a **Detector model name**, a **Description**, and the name of a **Role** \(that will be created for you\)\. Choose **Create a detector for each unique key value**\. \(If you want to create and use your own **Role**, follow the steps in [ Using the IAM Console to Manage Roles and Permissions](iotevents-start.md#iotevents-permissions-console) then select it as the **Role** here\.\)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/publish-detector-model.png)

1. Scroll down and enter a **Detector creation key** that is the name of one of the attributes of the input you defined earlier\. \(The attribute you choose as the "detector creation key" must be present in each message input, and it must be unique to each device that sends messages\. In this example, you use the **motorid** attribute\.\) Then choose **Save and publish**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/images/publish-detector-model-save.png)

At this point, you should make a backup copy of your detector model definition \(in JSON\) to use for recreating or updating the detector model, or as a template to begin creating a new detector model\. You can do this by using the following CLI command\. \(If necessary, change the name of the detector model to match what you used when you published it in the previous step\.\)\.

```
aws iotevents describe-detector-model  --detector-model-name motorDetectorModel > motorDetectorModel.json 
```

This creates a file \(`motorDetectorModel.json`\) that has contents similar to the following\.

```
{
    "detectorModel": {
        "detectorModelConfiguration": {
            "status": "ACTIVE", 
            "lastUpdateTime": 1552072424.212, 
            "roleArn": "arn:aws:iam::123456789012:role/IoTEventsRole", 
            "creationTime": 1552072424.212, 
            "detectorModelArn": "arn:aws:iotevents:us-west-2:123456789012:detectorModel/motorDetectorModel", 
            "key": "motorid", 
            "detectorModelName": "motorDetectorModel", 
            "detectorModelVersion": "1"
        }, 
        "detectorModelDefinition": {
            "states": [
                {
                    "onInput": {
                        "transitionEvents": [
                            {
                                "eventName": "Overpressurized", 
                                "actions": [
                                    {
                                        "setVariable": {
                                            "variableName": "pressureThresholdBreached", 
                                            "value": "$variable.pressureThresholdBreached + 3"
                                        }
                                    }
                                ], 
                                "condition": "$input.PressureInput.sensorData.pressure > 70", 
                                "nextState": "Dangerous"
                            }
                        ], 
                        "events": []
                    }, 
                    "stateName": "Normal", 
                    "onEnter": {
                        "events": [
                            {
                                "eventName": "init", 
                                "actions": [
                                    {
                                        "setVariable": {
                                            "variableName": "pressureThresholdBreached", 
                                            "value": "0"
                                        }
                                    }
                                ], 
                                "condition": "true"
                            }
                        ]
                    }, 
                    "onExit": {
                        "events": []
                    }
                }, 
                {
                    "onInput": {
                        "transitionEvents": [
                            {
                                "eventName": "Back to Normal", 
                                "actions": [], 
                                "condition": "$variable.pressureThresholdBreached <= 1 && $input.PressureInput.sensorData.pressure <= 70", 
                                "nextState": "Normal"
                            }
                        ], 
                        "events": [
                            {
                                "eventName": "Overpressurized", 
                                "actions": [
                                    {
                                        "setVariable": {
                                            "variableName": "pressureThresholdBreached", 
                                            "value": "3"
                                        }
                                    }
                                ], 
                                "condition": "$input.PressureInput.sensorData.pressure > 70"
                            }, 
                            {
                                "eventName": "Pressure Okay", 
                                "actions": [
                                    {
                                        "setVariable": {
                                            "variableName": "pressureThresholdBreached", 
                                            "value": "$variable.pressureThresholdBreached - 1"
                                        }
                                    }
                                ], 
                                "condition": "$input.PressureInput.sensorData.pressure <= 70"
                            }
                        ]
                    }, 
                    "stateName": "Dangerous", 
                    "onEnter": {
                        "events": [
                            {
                                "eventName": "Pressure Threshold Breached", 
                                "actions": [
                                    {
                                        "sns": {
                                            "targetArn": "arn:aws:sns:us-west-2:123456789012:MyIoTButtonSNSTopic"
                                        }
                                    }
                                ], 
                                "condition": "$variable.pressureThresholdBreached > 1"
                            }
                        ]
                    }, 
                    "onExit": {
                        "events": [
                            {
                                "eventName": "Normal Pressure Restored", 
                                "actions": [
                                    {
                                        "sns": {
                                            "targetArn": "arn:aws:sns:us-west-2:123456789012:IoTVirtualButtonTopic"
                                        }
                                    }
                                ], 
                                "condition": "true"
                            }
                        ]
                    }
                }
            ], 
            "initialStateName": "Normal"
        }
    }
}
```