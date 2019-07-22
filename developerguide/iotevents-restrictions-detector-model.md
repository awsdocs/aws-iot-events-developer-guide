# Detector Model Restrictions and Limitations<a name="iotevents-restrictions-detector-model"></a>

The following things are important to consider when creating a `detector model`\.

How to use the `actions` field:  
The `actions` field is a list of objects\. You can have more than one object, but only one action is allowed in each object\. For example:  

```
              "actions": [
                {
                  "setVariable": {
                    "variableName": "pressureThresholdBreached",
                    "value": "$variable.pressureThresholdBreached - 1"
                  }
                }
                {
                  "setVariable": {
                    "variableName": "temperatureIsTooHigh",
                    "value": "$variable.temperatureIsTooHigh - 1"
                  }
                }
              ]
```

How to use the `condition` field:  
The `condition` is required for `transitionEvents`\. It is optional in other cases\.  
If the `condition` field isn't present, it's equivalent to `"condition": true`\.   
The result of the evaluation of a condition expression should be a Boolean value\. If the result is not a Boolean value, it's equivalent to `false` and so won't trigger the `actions` or transition to the `nextState` specified in the event\.

Availability of variable values:  
If the value of a variable is set in an event, its new value isn't available or used to evaluate conditions in other events in the same group\. \(The new value isn't available or used an event condition in the same `onInput|onEnter|onExit` field\.\)  
The previous example shows this\. In the `"Dangerous"` state, in the `onInput` field, `"$variable.pressureThresholdBreached"` is decremented by one in the `"Pressure Okay"` event when the condition is met \(when the current input has pressure less than or equal to 70\)\.  

```
            {
              "eventName": "Pressure Okay",
              "condition": "$input.PressureInput.sensorData.pressure <= 70",
              "actions": [
                {
                  "setVariable": {
                    "variableName": "pressureThresholdBreached",
                    "value": "$variable.pressureThresholdBreached - 1"
                  }
                }
              ]
            }
```
The detector should transition back to the `"Normal"` state when `"$variable.pressureThresholdBreached"` reaches 0 \(that is, when the detector has received three contiguous pressure readings less than or equal to 70\)\. So, the `"BackToNormal"` event in `transitionEvents`, must test that `"$variable.pressureThresholdBreached"` is less than or equal to 1 \(not 0\), **and also** verify again that the current value given by `"$input.PressureInput.sensorData.pressure"` is less than or equal to 70\.  

```
          "transitionEvents": [
            {
              "eventName": "BackToNormal",
              "condition": "$input.PressureInput.sensorData.pressure <= 70 && $variable.pressureThresholdBreached <= 1",
              "nextState": "Normal"
            }
          ]
```
Otherwise, if the condition tests for only the value of the variable, two normal readings followed by an over\-pressure reading would fulfill the condition and transition back to the `"Normal"` state\. The condition will be looking at the value that `"$variable.pressureThresholdBreached"` was given during the previous time an input was processed\. \(Yes, the value of the variable is reset to 3 in the `"Overpressurized"` event, but remember that this new value is not yet available to any `condition`\.\)  
Every time control enters the `onInput` field, any `condition` only sees the value of a variable as it was when we started processing the input, before it's changed by any actions specified in `onInput`\. The same is true for `onEnter` and `onExit`—any change made to a variable when we enter or exit the state is not available to other conditions specified in the same `onEnter` or `onExit` fields\.

Latency when updating a detector model:  
If you update \(or delete and recreate\) a detector model \(see [ UpdateDetectorModel](iotevents-commands.md#api-iotevents-UpdateDetectorModel)\), there is some delay before all spawned detectors \(instances\) are deleted and the new model is used to recreate the detectors\. \(They will be recreated once the new detector model takes effect and new inputs arrive\.\) During this time inputs may continue to be processed by the detectors spawned by the previous version of the detector model\. During this period, you might continue to receive alerts defined by the previous detector model\.

Spaces in input keys:  
Spaces are allowed in input keys, but references to the key must be enclosed in backticks, both in the definition of the input attribute and when the value of the key is referenced in an expression\. For example, given a message payload like the following:  

```
{
  "motor id": "A32",
  "sensorData" {
    "motor pressure": 56,
    "motor temperature": 39
  }
}
```
Use the following to define the input\.  

```
{
  "inputName": "PressureInput",
  "inputDescription": "Pressure readings from a motor",
  "inputDefinition": {
    "attributes": [
      { "jsonPath": "sensorData.`motor pressure`" },
      { "jsonPath": "`motor id`" }
    ]
  }
}
```
In a conditional expression, you must refer to the value of any such key using backticks also\.  

```
$input.PressureInput.sensorData.`motor pressure`
```