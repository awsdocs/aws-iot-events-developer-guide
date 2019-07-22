# AWS IoT Events Service Limits<a name="iotevents-limits"></a>


****  

| Resource | Description | Limit | Adjustable? | 
| --- | --- | --- | --- | 
| Detector models per input | The maximum number of detector models that can be associated with a single input\. | 10 | no | 
| Message size | The maximum size of a message \(in Kilobytes\)\. | 10 | yes | 
| Messages per detector per second | The maximum number of messages that can be sent to a detector in a second\. | 10 | no | 
| Detectors per detector model | The maximum number of detectors that can be created by a detector model\. | 100,000 | yes | 
| Detector model definition size | The maximum size \(in Kilobytes\) of a detector model definition\. | 512 | no | 
| Detector models | The maximum number of detector models for this account\. | 50 | yes | 
| Detector model versions | The maximum number of versions of a single detector model for this account\. | 500 | yes | 
| Inputs | The maximum number of inputs for this account\. | 50 | yes | 
| Trigger expressions | The maximum number of trigger expressions per state\. | 20 | yes | 
| State variables per detector model definition | The maximum number of state variables in a detector model definition\. | 50 | yes | 
| Timers scheduled per detector | The maximum number of timers that can be scheduled by a detector\. | 5 | yes | 


| API | Limit Description | Adjustable? | 
| --- | --- | --- | 
| BatchPutMessage | 1000 transactions per second | yes | 
+ All names for detector models and inputs must be unique within an account\.
+ Detector model and input names can't be changed once created\.