# Detector model analysis and diagnostic information<a name="analyze-diagnostic-information"></a>

Detector model analyses gather the following diagnostic information:
+ **Level** – The severity level of the analysis result\. Based on the severity level, analysis results fall into three general categories:
  + **Information** \(`INFO`\) – An information result tells you about a significant field in your detector model\. This type of result usually doesn't require immediate action\.
  + **Warning** \(`WARNING`\) – A warning result draws special attention to fields that might cause issues for your detector model\. We recommend that you review warnings and take necessary actions before you use your detector model in production environments\. Otherwise, the detector model might not work as expected\.
  + **Error** \(`ERROR`\) – An error result notifies you about a problem found in your detector model\. AWS IoT Events automatically performs this set of analyses when you try to publish the detector model\. You must fix all errors before you can publish the detector model\.
+ **Location** – Contains information that you can use to locate the field in your detector model that the analysis result references\. A location typically includes the state name, transition event name, event name, and expression \(for example, `in state TemperatureCheck in onEnter in event Init in action setVariable`\)\.
+ **Type** – The type of the analysis result\. Analysis types fall into the following categories:
  + `supported-actions` – AWS IoT Events can invoke actions when a specified event or transition event is detected\. You can define built\-in actions to use a timer or set a variable, or send data to other AWS services\. You must specify actions that work with other AWS services in an AWS Region where the AWS services are available\. 
  + `service-limits` – Service quotas, also known as limits, are the maximum or minimum number of service resources or operations for your AWS account\. Unless otherwise noted, each quota is Region\-specific\. Depending on your business needs, you can update your detector model to avoid encountering limits or request a quota increase\. You can request increases for some quotas, and other quotas can't be increased\. For more information, see [Quotas](https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-quotas.html)\.
+ **`structure`** – The detector model must have all required components such as states and follow a structure that AWS IoT Events supports\. A detector model must have at least one state and a condition that evaluates the incoming input data to detect significant events\. When an event is detected, the detector model transitions to the next state and can invoke actions\. These events are known as transition events\. A transition event must direct the next state to enter\.
+ **`expression-syntax`** – AWS IoT Events provides several ways to specify values when you create and update detector models\. You can use literals, operators, functions, references, and substitution templates in the expressions\. You can use expressions to specify literal values, or AWS IoT Events can evaluate the expressions before you specify particular values\. Your expression must follow the required syntax\. For more information, see [Expressions](iotevents-expressions.md)\.

  Detector Model expressions in AWS IoT Events can reference specific data or a resource\.
  + **`data-type`** – AWS IoT Events supports integer, decimal, string, and Boolean data types\. If AWS IoT Events can automatically convert the data of one data type to another during expression evaluation, these data types are compatible\.
**Note**  
Integer and decimal are the only compatible data types supported by AWS IoT Events\.
AWS IoT Events can't evaluate arithmetic expressions because AWS IoT Events can't convert an integer to a string\.
  + **`referenced-data`** – You must define the data referenced in your detector model before you can use the data\. For example, if you want to send data to a DynamoDB table, you must define a variable that references the table name before you can use the variable in an expression \(`$variable.TableName`\)\.
  + **`referenced-resource`** – Resources that the detector model uses must be available\. You must define resources before you can use them\. For example, you want to create a detector model to monitor the temperature of a greenhouse\. You must define an input \(`$input.TemperatureInput`\) to route incoming temperature data to your detector model before you can use the `$input.TemperatureInput.sensorData.temperature` to reference the temperature\.

See the following section to troubleshoot errors and find possible solutions from the analysis of your detector model\.

## Troubleshoot detector model errors<a name="troubleshoot-error-messages"></a>

The types of errors described above provide diagnostic information about a detector model and correspond to messages that you might retrieve\. Use these messages and suggested solutions to troubleshoot errors with your detector model\.

**Topics**
+ [`Location`](#analyze-location)
+ [`supported-actions`](#analyze-supported-actions)
+ [`service-limits`](#analyze-service-limits)
+ [`structure`](#analyze-structure)
+ [`expression-syntax`](#analyze-expression-syntax)
+ [`data-type`](#analyze-data-type)
+ [`referenced-data`](#analyze-referenced-data)
+ [`referenced-resource`](#analyze-referenced-resource)

### `Location`<a name="analyze-location"></a>

An analysis result with information about `Location`, corresponds to the following error message: 
+ **Message** – Contains additional information about the analysis result\. This can be an information, warning, or error message\.

#### <a name="w28aac35b9c13b9b7b5b1b3b1b1"></a>

   You might receive this error message if you specified an action that AWS IoT Events currently doesn't support\. For a list of supported actions, see [Supported actions](iotevents-supported-actions.md)\.

### `supported-actions`<a name="analyze-supported-actions"></a>

An analysis result with information about `supported‐actions`, corresponds to the following error messages: 
+ **Message:** Invalid action type present in action definition: *action\-definition*\.

#### <a name="w28aac35b9c13b9b9b5b1b3b1b1"></a>

   You might receive this error message if you specified an action that AWS IoT Events currently doesn't support\. For a list of supported actions, see [Supported actions](iotevents-supported-actions.md)\.
+ **Message:** DetectorModel definition has an *aws\-service* action, but the *aws\-service* service is not supported in the regionregion *region\-name*\.

#### <a name="w28aac35b9c13b9b9b5b3b3b1b1"></a>

   You might receive this error message if the action that you specified is supported by AWS IoT Events, but the action isn't available in your current Region\. This might occur when you try to send data to an AWS service that isn't available in the Region\. You must also choose the same Region for both AWS IoT Events and the AWS services that you're using\.

### `service-limits`<a name="analyze-service-limits"></a>

An analysis result with information about `service‐limits`, corresponds to the following error messages: 
+ **Message:** Content Expression allowed in payload exceeded the limit *content\-expression\-size* bytes in event *event\-name* in state *state\-name*\.

#### <a name="w28aac35b9c13b9c11b5b1b3b1b1"></a>

   You might receive this error message if the content expression for your action payload is greater than 1024 bytes\. The size of the content expression for a payload can be up to 1024 bytes\.
+ **Message:** Number of states allowed in detector model definition exceeded the limit *states\-per\-detector\-model*\.

#### <a name="w28aac35b9c13b9c11b5b3b3b1b1"></a>

   You might receive this error message if your detector model has more than 20 states\. A detector model can have up to 20 states\.
+ **Message:** The duration for timer *timer\-name* should be at least *minimum\-timer\-duration* seconds long\.

#### <a name="w28aac35b9c13b9c11b5b5b3b1b1"></a>

   You might receive this error message if the duration of your timer is less than 60 seconds\. We recommend that the duration of a timer is between 60 and 31622400 seconds\. If you specify an expression for the duration of your timer, the evaluated result of the duration expression is rounded down to the nearest whole number\.
+ **Message:** Number of actions allowed per event exceeded the limit *actions\-per\-event* in detector model definition

#### <a name="w28aac35b9c13b9c11b5b7b3b1b1"></a>

   You might receive this error message if the event has more than 10 actions\. You can have up to 10 actions for each event in your detector model\.
+ **Message:** Number of transition events allowed per state exceeded the limit *transition\-events\-per\-state* in detector model definition\.

#### <a name="w28aac35b9c13b9c11b5b9b3b1b1"></a>

   You might receive this error message if the state has more than 20 transition events\. You can have up to 20 transition events for each state in your detector model\.
+ **Message:** Number of events allowed per state exceeded the limit *events\-per\-state* in detector model definition

#### <a name="w28aac35b9c13b9c11b5c11b3b1b1"></a>

   You might receive this error message if the state has more than 20 events\. You can have up to 20 events for each state in your detector model\.
+ **Message:** The maximum number of detector models that can be associated with a single input may have reached the limit\. Input *input\-name* is used in *detector\-models\-per\-input* detector model routes\.

#### <a name="w28aac35b9c13b9c11b5c13b3b1b1"></a>

   You might receive this warning message if you tried to route an input to more than 10 detector models\. You can have up to 10 different detector models associated with a single detector model\.

### `structure`<a name="analyze-structure"></a>

An analysis result with information about `structure`, corresponds to the following error messages: 
+ **Message:** Actions may only have one type defined, but found an action with *number\-of\-types* types\. Please split into separate Actions\.

#### <a name="w28aac35b9c13b9c13b5b1b3b1b1"></a>

   You might receive this error message if you specified two or more actions in a single field by using API operations to create or update your detector model\. You can define an array of `Action` objects\. Make sure that you define each action as a separate object\.
+ **Message:** The TransitionEvent *transition\-event\-name* transitions to a non\-existent state *state\-name*\.

#### <a name="w28aac35b9c13b9c13b5b3b3b1b1"></a>

   You might receive this error message if AWS IoT Events couldn't find the next state that your transition event referenced\. Make sure that the next state is defined and that you entered the correct state name\.
+ **Message:** The DetectorModelDefinition had a shared state name: found state *state\-name* with *number\-of\-states* repetitions\.

#### <a name="w28aac35b9c13b9c13b5b5b3b1b1"></a>

   You might receive this error message if you use the same name for one or more states\. Make sure that you give a unique name to each state in your detector model\. The state name must have 1\-128 characters\. Valid characters: a\-z, A\-Z, 0\-9, \_ \(underscore\), and \- \(hyphen\)\.
+ **Message:** The Definition's initialStateName *initial\-state\-name* did not correspond to a defined State\.

#### <a name="w28aac35b9c13b9c13b5b7b3b1b1"></a>

   You might receive this error message if the initial state name is incorrect\. The detector model remains in the initial \(start\) state until an input arrives\. Once an input arrives, the detector model immediately transitions to the next state\. Make sure that the initial state name is the name of a defined state and that you enter the correct name\.
+ **Message:** Detector Model Definition must use at least one Input in a condition\.

#### <a name="w28aac35b9c13b9c13b5b9b3b1b1"></a>

   You might receive this error if you didn't specify an input in a condition\. You must use at least one input in at least one condition\. Otherwise, AWS IoT Events doesn't evaluate incoming data\.
+ **Message:** Only one of seconds and durationExpression can be set in SetTimer\.

#### <a name="w28aac35b9c13b9c13b5c11b3b1b1"></a>

   You might receive this error message if you used both `seconds` and `durationExpression` for your timer\. Make sure that you use either `seconds` or `durationExpression` as the parameters of `SetTimerAction`\. For more information, see [SetTimerAction](https://docs.aws.amazon.com/iotevents/latest/apireference/API_SetTimerAction.html) in the *AWS IoT Events API Reference*\.

### `expression-syntax`<a name="analyze-expression-syntax"></a>

An analysis result with information about `expression‐syntax`, corresponds to the following error messages: 
+ **Message:** Your payload expression \{*expression*\} isn't valid\. The defined payload type is JSON, so you must specify an expression that AWS IoT Events would evaluate to a string\.

#### <a name="w28aac35b9c13b9c15b5b1b3b1b1"></a>

   If the specified payload type is JSON, AWS IoT Events first checks if the service can evaluate your expression to a string\. The evaluated result can't be a Boolean or number\. If the validation doesn't succeed, you might receive this error\.
+ **Message:** `SetVariableAction.value` must be an expression\. Failed to parse value '*variable\-value*'

#### <a name="w28aac35b9c13b9c15b5b3b3b1b1"></a>

   You can use `SetVariableAction` to define a variable with a `name` and `value`\. The `value` can be a string, number, or Boolean value\. You can also specify an expression for the `value`\. For more information, see [SetVariableAction](https://docs.aws.amazon.com/iotevents/latest/apireference/API_SetVariableAction.html), in the *AWS IoT Events API Reference*\.
+ **Message:** We couldn't parse your expression of the attributes \(*attribute\-name*\) for the DynamoDB action\. Enter expression with the correct syntax\.

#### <a name="w28aac35b9c13b9c15b5b5b3b1b1"></a>

   You must use expressions for all parameters in `DynamoDBAction`\. substitution templates\. For more information, see [DynamoDBAction](https://docs.aws.amazon.com/iotevents/latest/apireference/API_DynamoDBAction.html) in the *AWS IoT Events API Reference*\.
+ **Message:** We couldn't parse your expression of the tableName for the DynamoDBv2 action\. Enter expression with the correct syntax\.

#### <a name="w28aac35b9c13b9c15b5b7b3b1b1"></a>

   The `tableName` in `DynamoDBv2Action` must be a string\. You must use an expression for the `tableName`\. The expressions accept literals, operators, functions, references, and substitution templates\. For more information, see [DynamoDBv2Action](https://docs.aws.amazon.com/iotevents/latest/apireference/API_DynamoDBv2Action.html) in the *AWS IoT Events API Reference*\.
+ **Message:** We couldn't evaluate your expression to valid JSON\. The DynamoDBv2 action only supports the JSON payload type\.

#### <a name="w28aac35b9c13b9c15b5b9b3b1b1"></a>

   The payload type for `DynamoDBv2` must be JSON\. Make sure that AWS IoT Events can evaluate your content expression for the payload to valid JSON\. For more information, see [DynamoDBv2Action](https://docs.aws.amazon.com/iotevents/latest/apireference/API_DynamoDBv2Action.html), in the *AWS IoT Events API Reference*\. 
+ **Message:** We couldn't parse your content expression for the payload of *action\-type*\. Enter a content expression with the correct syntax\.

#### <a name="w28aac35b9c13b9c15b5c11b3b1b1"></a>

   The content expression can contain strings \('*string*'\), variables \($variable\.*variable\-name*\), input values \($input\.*input\-name*\.*path\-to\-datum*\), string concatenations, and strings that contain `${}`\. 
+ **Message:** Customized Payloads must be non\-empty\.

#### <a name="w28aac35b9c13b9c15b5c13b3b1b1"></a>

   You might receive this error message, if you chose **Custom payload** for your action and didn't enter a content expression in the AWS IoT Events console\. If you choose **Custom payload**, you must enter a content expression under **Custom payload**\. For more information, see [Payload](https://docs.aws.amazon.com/iotevents/latest/apireference/API_Payload.html) in the *AWS IoT Events API Reference*\.
+ **Message:** Failed to parse duration expression '*duration\-expression*' for timer '*timer\-name*'\.

#### <a name="w28aac35b9c13b9c15b5c15b3b1b1"></a>

   The evaluated result of your duration expression for the timer must be a value between 60–31622400\. The evaluated result of the duration is rounded down to the nearest whole number\.
+ **Message:** Failed to parse expression '*expression*' for *action\-name*

#### <a name="w28aac35b9c13b9c15b5c17b3b1b1"></a>

   You might receive this message if the expression for the specified action has incorrect syntax\. Make sure that you enter an expression with the correct syntax\. For more information, see [Syntax](iotevents-expressions.md#expression-syntax)\.
+ **Message:** Your *fieldName* for `IotSitewiseAction` couldn't be parsed\. You must use correct syntax in your expression\.

#### <a name="w28aac35b9c13b9c15b5c19b3b1b1"></a>

   You might receive this error if AWS IoT Events couldn't parse your *fieldName* for `IotSitewiseAction`\. Make sure the *fieldName* uses an expression that AWS IoT Events can parse\. For more information, see [IotSiteWiseAction](https://docs.aws.amazon.com/iotevents/latest/apireference/API_IotSiteWiseAction.html) in the *AWS IoT Events API Reference*\.

### `data-type`<a name="analyze-data-type"></a>

An analysis result with information about `data‐type`, corresponds to the following error messages: 
+ **Message:** Duration expression *duration\-expression* for timer *timer\-name* is not valid, it must return a number\.

#### <a name="w28aac35b9c13b9c17b5b1b3b1b1"></a>

   You might receive this error message if AWS IoT Events couldn't evaluate the duration expression for your timer to a number\. Make sure that your `durationExpression` can be converted to a number\. Other data types, such as Boolean, aren't supported\. 
+ **Message:** Expression *condition\-expression* is not a valid condition expression\.

#### <a name="w28aac35b9c13b9c17b5b3b3b1b1"></a>

   You might receive this error message if AWS IoT Events couldn't evaluate your `condition-expression` to a Boolean value\. The Boolean value must be either `TRUE` or `FALSE`\. Make sure that your condition expression can be converted to a Boolean value\. If the result isn't a Boolean value, it's equivalent to `FALSE` and doesn't invoke the actions or transition to the `nextState` specified in the event\.
+ **Message:** Incompatible data types \[*inferred\-types*\] found for *reference* in the following expression: *expression*

#### <a name="w28aac35b9c13b9c17b5b5b3b1b1"></a>

   **Solution**: All expressions for the same input attribute or variable in the detector model must reference the same data type\. 

  Use the following information to resolve the issue:<a name="expression-reference-type-compatibility"></a>
  + When you use a reference as an operand with one or more operators, make sure that all data types that you reference are compatible\.

    For example, in the following expression, integer `2` is an operand of both the `==` and `&&` operators\. To ensure that the operands are compatible, `$variable.testVariable + 1` and `$variable.testVariable` must reference an integer or decimal\.

    In addition, integer `1` is an operand of the `+` operator\. Therefore, `$variable.testVariable` must reference an integer or decimal\.

    ```
    ‘$variable.testVariable + 1 == 2 && $variable.testVariable’
    ```
  + When you use a reference as an argument passed to a function, make sure that the function supports the data types that you reference\.

    For example, the following `timeout("time-name")` function requires a string with double quotes as the argument\. If you use a reference for the *timer\-name* value, you must reference a string with double quotes\.

    ```
    timeout("timer-name")
    ```
**Note**  
For the `convert(type, expression)` function, if you use a reference for the *type* value, the evaluated result of your reference must be `String`, `Decimal`, or `Boolean`\.

  For more information, see [References](iotevents-expressions.md#expression-reference)\.
+ **Message:** Incompatible data types \[*inferred\-types*\] used with *reference*\. This may lead to a runtime error\.

#### <a name="w28aac35b9c13b9c17b5b7b3b1b1"></a>

   You might receive this warning message if two expressions for the same input attribute or variable reference two data types\. Make sure that your expressions for the same input attribute or variable reference the same data type in the detector model\.
+ **Message:** The data types \[*inferred‐types*\] that you entered for the operator \[*operator*\] aren't compatible for the following expression: '*expression*'

#### <a name="w28aac35b9c13b9c17b5b9b3b1b1"></a>

   You might receive this error message if your expression combines data types that are not compatible with a specified operator\. For example, in the following expression, the operator `+` is compatible with Integer, Decimal, and String data types, but not operands of Boolean data type\.

  ```
  true + false
  ```

  You must make sure that the data types you use with an operator are compatible\.
+ **Message:** The data types \[*inferred‐types*\] found for *input‐attribute* aren't compatible and can lead to a runtime error\.

#### <a name="w28aac35b9c13b9c17b5c11b3b1b1"></a>

   You might receive this error message if two expressions for the same input attribute reference two data types for either the `OnEnterLifecycle` of a state, or for both the `OnInputLifecycle` and `OnExitLifecycle` of a state\. Make sure your expressions in `OnEnterLifecycle` \(or, both `OnInputLifecycle` and `OnExitLifecycle`\) reference the same data type for each state of your detector model\.
+ **Message:** The payload expression \[*expression*\] isn't valid\. Specify an expression that would evaluate to a string at runtime because the payload type is JSON format\.

#### <a name="w28aac35b9c13b9c17b5c13b3b1b1"></a>

   You might receive this error if your specified payload type is JSON, but AWS IoT Events can't evaluate its expression to a String\. Make sure the evaluated result is a String, not a Boolean or a number\. 
+ **Message:** Your interpolated expression \{*interpolated\-expression*\} must evaluate to either an integer or a Boolean value at runtime\. Otherwise, your payload expression \{*payload\-expression*\} won't be parseable at runtime as valid JSON\.

#### <a name="w28aac35b9c13b9c17b5c15b3b1b1"></a>

   You might receive this error message if AWS IoT Events couldn't evaluate your interpolated expression to an integer or a Boolean value\. Make sure your interpolated expression can be converted to an integer or a Boolean value, because other data types, such as tring, aren't supported\.
+ **Message:** The expression type in the `IotSitewiseAction` field *expression* is defined as type *defined‐type* and inferred as type *inferred‐type*\. The defined type and the inferred type must be the same\.

#### <a name="w28aac35b9c13b9c17b5c17b3b1b1"></a>

   You might receive this error message if your expression in the `propertyValue` of `IotSitewiseAction` has a data type defined differently than the data type inferred by AWS IoT Events\. Make sure you use the same data type for all instances of this expression in your detector model\.
+ **Message:** The data types \[*inferred\-types*\] used for `setTimer` action don't evaluate to `Integer` for the following expression: *expression*

#### <a name="w28aac35b9c13b9c17b5c19b3b1b1"></a>

   You might receive this error message if the inferred data type for your duration expression isn't Integer or Decimal\. Make sure your `durationExpression` can be converted to a number\. Other data types, such as Boolean and String, aren't supported\.
+ **Message:** The data types \[*inferred\-types*\] used with operands of the comparison operator \[*operator*\] are not compatible in the following expression: *expression*

#### <a name="w28aac35b9c13b9c17b5c21b3b1b1"></a>

   The inferred data types for the operands of the *operator* in the conditional expression \(*expression*\) of your detector model don't match\. The operands must be used with the matching data types in all other parts of your detector model\.

**Tip**  
You can use `convert` to change the data type of an expression in your detector model\. For more information, see [Functions](iotevents-expressions.md#expression-function)\.

### `referenced-data`<a name="analyze-referenced-data"></a>

An analysis result with information about `referenced‐data`, corresponds to the following error messages: 
+ **Message:** Detected broken Timer: timer *timer\-name* is used in an expression but is never set\.

#### <a name="w28aac35b9c13b9c19b5b1b3b1b1"></a>

   You might receive this error message if you use a timer that isn't set\. You must set a timer before you use it in an expression\. Also, make sure that you enter the correct timer name\.
+ **Message:** Detected broken Variable: variable *variable\-name* is used in an expression but is never set\.

#### <a name="w28aac35b9c13b9c19b5b3b3b1b1"></a>

   You might receive this error message if you use a variable that isn't set\. You must set a variable before you use it in an expression\. Also, make sure that you enter the correct variable name\.

### `referenced-resource`<a name="analyze-referenced-resource"></a>

An analysis result with information about `referenced‐resource`, corresponds to the following error messages: 
+ **Message:** Detector Model Definition contains reference to Input that does not exist\.

#### <a name="w28aac35b9c13b9c21b5b1b3b1b1"></a>

   You might receive this error message if you use expressions to reference an input that doesn't exist\. Make sure that your expression references an existing input and enter the correct input name\. If you don't have an input, create one first\.
+ **Message:** Detector Model Definition contains invalid InputName: *input\-name*

#### <a name="w28aac35b9c13b9c21b5b3b3b1b1"></a>

   You might receive this error message if your detector model contains an invalid input name\. Make sure that you enter the correct input name\. The input name must have 1\-128 characters\. Valid characters: a\-z, A\-Z, 0\-9, \_ \(underscore\), and \- \(hyphen\)\.