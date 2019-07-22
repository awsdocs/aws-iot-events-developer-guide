# Device HeartBeat<a name="iotevents-examples-dhb"></a>

This detector model is one of the templates available from the AWS IoT Events console\. It's included here for your convenience\.

```
{
    "detectorModelName": "DeviceHeartBeat", 
    "detectorModelDefinition": {
        "states": [
            {
                "onInput": {
                    "transitionEvents": [
                        {
                            "eventName": "To_normal", 
                            "actions": [], 
                            "condition": "$input.AWS_IoTEvents_Blueprints_Heartbeat_Input.value > 0", 
                            "nextState": "Normal"
                        }
                    ], 
                    "events": []
                }, 
                "stateName": "Offline", 
                "onEnter": {
                    "events": [
                        {
                            "eventName": "Send_notification", 
                            "actions": [
                                {
                                    "sns": {
                                        "targetArn": "arn:aws:sns:us-west-2:123456789012:Offline"
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
                            "eventName": "Go_offline", 
                            "actions": [], 
                            "condition": "timeout(\"awake\")", 
                            "nextState": "Offline"
                        }
                    ], 
                    "events": [
                        {
                            "eventName": "Reset_timer", 
                            "actions": [
                                {
                                    "resetTimer": {
                                        "timerName": "awake"
                                    }
                                }
                            ], 
                            "condition": "true"
                        }
                    ]
                }, 
                "stateName": "Normal", 
                "onEnter": {
                    "events": [
                        {
                            "eventName": "Create_timer", 
                            "actions": [
                                {
                                    "setTimer": {
                                        "seconds": 300, 
                                        "timerName": "awake"
                                    }
                                }
                            ], 
                            "condition": "$input.AWS_IoTEvents_Blueprints_Heartbeat_Input.value > 0"
                        }
                    ]
                }, 
                "onExit": {
                    "events": []
                }
            }
        ], 
        "initialStateName": "Normal"
    }
}
```