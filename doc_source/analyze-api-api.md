# Analyzing a detector model \(AWS CLI\)<a name="analyze-api-api"></a>

The following steps use the AWS CLI to analyze a detector model\.

1. Run the following command to start an analysis\.

   ```
   aws iotevents start-detector-model-analysis --cli-input-json file://file-name.json
   ```
**Note**  
Replace *file\-name* with the name of the file that contains the detector model definition\.  
**Example Detector model definition**  

   ```
   {
       "detectorModelDefinition": {
           "states": [
               {
                   "stateName": "TemperatureCheck",
                   "onInput": {
                       "events": [
                           {
                               "eventName": "Temperature Received",
                               "condition": "isNull($input.TemperatureInput.sensorData.temperature)==false",
                               "actions": [
                                   {
                                       "iotTopicPublish": {
                                           "mqttTopic": "IoTEvents/Output"
                                       }
                                   }
                               ]
                           }
                       ],
                       "transitionEvents": []
                   },
                   "onEnter": {
                       "events": [
                           {
                               "eventName": "Init",
                               "condition": "true",
                               "actions": [
                                   {
                                       "setVariable": {
                                           "variableName": "temperatureChecked",
                                           "value": "0"
                                       }
                                   }
                               ]
                           }
                       ]
                   },
                   "onExit": {
                       "events": []
                   }
               }
           ],
           "initialStateName": "TemperatureCheck"
       }
   }
   ```

   If you use the AWS CLI to analyze an existing detector model, choose one of the following to retrieve the detector model definition:
   + If you want to use the AWS IoT Events console, do the following:

     1. In navigation pane, choose **Detector models**\.

     1. Under **Detector models**, choose the target detector model\.

     1. Choose **Export detector model** from **Action** to download the detector model\. The detector model is saved in JSON\.

     1. Open the detector model JSON file\.

     1. You only need the `detectorModelDefinition` object\. Remove the following:
        + The first curly bracket \(`{`\) at the top of the page
        + The `detectorModel` line
        + The `detectorModelConfiguration` object
        + The last curly bracket \(`}`\) at the bottom of the page

     1. Save the file\.
   + If you want to use the AWS CLI, do the following:

     1. Run the following command in a terminal\.

        ```
        aws iotevents describe-detector-model --detector-model-name detector-model-name
        ```

     1. Replace *detector\-model\-name* with the name of your detector model\.

     1. Copy the `detectorModelDefinition` object to a text editor\.

     1. Add curly brackets \(`{}`\) outside of the `detectorModelDefinition`\.

     1. Save the file in JSON\.  
**Example response**  

   ```
   {
       "analysisId": "c1133390-14e3-4204-9a66-31efd92a4fed"
   }
   ```

1. Copy the analysis ID from the output\.

1. Run the following command to retrieve the status of the analysis\.

   ```
   aws iotevents describe-detector-model-analysis --analysis-id "analysis-id"
   ```
**Note**  
Replace *analysis\-id* with the analysis ID that you copied\.  
**Example response**  

   ```
   {
       "status": "COMPLETE"
   }
   ```

   The status can be one of the following values:
   + `RUNNING` – AWS IoT Events is analyzing your detector model\. This process can take up to one minute to complete\.
   + `COMPLETE` – AWS IoT Events finished analyzing your detector model\.
   + `FAILED` – AWS IoT Events couldn't analyze your detector model\. Try again later\.

1. Run the following command to retrieve one or more analysis results of the detector model\.
**Note**  
Replace *analysis\-id* with the analysis ID that you copied\.

   ```
   aws iotevents get-detector-model-analysis-results --analysis-id "analysis-id"
   ```  
**Example response**  

   ```
   {
       "analysisResults": [
           {
               "type": "data-type",
               "level": "INFO",
               "message": "Inferred data types [Integer] for $variable.temperatureChecked",
               "locations": []
           },
           {
               "type": "referenced-resource",
               "level": "ERROR",
               "message": "Detector Model Definition contains reference to Input 'TemperatureInput' that does not exist.",
               "locations": [
                   {
                       "path": "states[0].onInput.events[0]"
                   }
               ]
           }
       ]
   }
   ```

**Note**  
After AWS IoT Events starts analyzing your detector model, you have up to 24 hours to retrieve the analysis results\.