# Troubleshooting AWS IoT Events<a name="iotevents-troubleshooting"></a>

The following information might help you troubleshoot common issues in AWS IoT Events\.

**Q: **I deleted or updated a detector model a few minutes ago, why am I still getting state updates from the old detector model via MQTT messaging or SNS alerts?  
**A: **If you update or republish a detector model \(see [ UpdateDetectorModel](iotevents-commands.md#api-iotevents-UpdateDetectorModel)\), there is some delay before all spawned detectors \(instances\) are deleted and the new model is used to recreate the detectors\. \(They will be recreated when the new detector model takes effect and as new inputs arrive\.\) During this time, inputs may continue to be processed by the detectors spawned by the previous version of the detector model\. You might continue to receive alerts defined by the previous detector model during this period\. Wait for at least seven minutes before you recheck the deletion or update, or before you report an error\.

**Q: **I specified an event to trigger an action \(or transition to a new state\), but the action \(transition\) doesn't happen when the condition is met\.  
**A: **Check your event's condition expression to make sure the result of the evaluation of the expression is a Boolean value\. If the result is not a Boolean value, it's equivalent to `false` and so will not trigger the `actions` or transition to the `nextState` specified in the event\.

**Q: **I'm getting errors when creating a `detector model`\. What's wrong?  
**A: **Keep the following limitations in mind when creating a `detector model`:  
\- Only one action is allowed in each `action` field\.   
\- The `condition` is required for `transitionEvents`\. It is optional in other cases\.  
\- If the `condition` field isn't present, it's equivalent to `"condition": true`\.   
\- The result of the evaluation of a condition expression should be a Boolean value\. If the result is not a Boolean value, it's equivalent to `false` and so will not trigger the `actions` or transition to the `nextState` specified in the event\.

**Q: **I defined a condition to trigger an event or a transition event when a variable reaches a certain value, but it doesn't work \(or doesn't work until the value of the variable is more or less than I expect\)\. What's wrong?   
**A: **If the value of a variable is set \(`setVariable`\) in any `event` defined within an `onInput;onEnter;onExit` field, its new value isn't used when evaluating any `condition` during that processing cycle \(that is, during the processing of the input, or during entry or exit\)\. Instead, the current value \(if any\) is used throughout that cycle\. 

**Q: **My detectors are entering the wrong states given the inputs \(messages\) they ingest\. What's wrong?  
**A: **If you send multiple messages to inputs using `BatchPutMessage`, the order in which the messages/inputs are processed isn't guaranteed\. To guarantee ordering, send messages one at a time, and wait each time for `BatchPutMessage` to acknowledge success\.

**Q: **When I call \(invoke\) an API, I get a 'Connection aborted' error\. What's wrong?  
**A: **If you are using TLS 1\.0 to establish the connection, you may see an error like the following\.  

```
('Connection aborted.', error(54, 'Connection reset by peer'))
```
Verify that OpenSSL is using TLS 1\.1 or TLS 1\.2\. This should already be the default under most Linux distributions or Windows version 7 and later\. Users of macOS might need to upgrade OpenSSL, and might find the blog entry [How to upgrade OpenSSL \(macOS\)](https://medium.com/@katopz/how-to-upgrade-openssl-8d005554401) useful\.