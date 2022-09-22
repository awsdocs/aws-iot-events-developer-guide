# Create an input<a name="create-input-overview"></a>

When you construct the inputs for your models, we recommend gathering files that contain sample message payloads that your devices or processes will send to report their health status\. Having these files will help you define the inputs that are required\. 

You can create an input through multiple methods that are described in this section\.

To get started, create a file named `input.json` on your local file system with the following contents: 

```
{
  "motorid": "Fulton-A32",
  "sensorData": {
    "pressure": 23,
    "temperature": 47
  }
}
```

Now that you have this starter `input.json` file, you can create an input\. Use one of the topics in this section for instructions about creating an input by using the navigation pane or by using the detector model\. 

**Topics**
+ [Create an input in the Navigation Pane](create-input-for-models.md)
+ [Create an input in the Detector Model](iotevents-detector-input.md)