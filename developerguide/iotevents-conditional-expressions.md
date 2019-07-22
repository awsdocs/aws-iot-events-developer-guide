# Conditional Expression Syntax<a name="iotevents-conditional-expressions"></a>

The following syntax is used in conditional expressions \(see the field `detectorDefinition.states.onInput|onEnter|onExit.events.condition` in the input to the [ CreateDetectorModel](iotevents-commands.md#api-iotevents-CreateDetectorModel) API\)\.

**Note**  
The result of the evaluation of a condition expression should be a Boolean value\. If the result isn't a Boolean value, it is equivalent to `false` and won't trigger the `actions` or transition to the `nextState` specified as part of the event in which it occurs\.

**Literals:**
+ Integer
+ Decimal
+ String
+ Boolean

**Operators:**
+ 

**Unary:**
  + Not: `!`
  + Minus: `-`
+ 

**Arithmetic:**
  + Addition: `+`

    For strings, `+` performs concatenation: 'my' \+ 'string' = 'mystring'
  + Subtraction: `-`
  + Division: `/`

    The result of the division will be a rounded integer value unless at least one of the divisor or dividend is a decimal value\.
  + Multiplication: `*`
+ 

**Boolean:**
  + Less Than: `<`
  + Less Than Or Equal To: `<=`
  + Equal To: `==`
  + Not Equal To: `!=`
  + Greater Than Or Equal To: `>=`
  + Greater Than: `>`
  + AND: `&&`
  + OR: `||`

    Note that when a subexpression of `||` contains undefined data, that subexpression is treated as `FALSE`\.
+ 

**Parentheses:**
  + You can use parentheses to group terms within an expression\.

**Built\-In Functions:**
+ `timeout("timer-name")`

  Evaluates to `TRUE` if the specified timer has elapsed\. Replace `"timer-name"` with the name of a timer you have defined, in quotation marks\. In an event action, you can define a timer and set it running, or reset or clear one you have previously defined\. See the field `detectorModelDefinition.states.onInput|onEnter|onExit.events.actions.setTimer.timerName`\. A timer set in one state can be referenced in another\. But you must ensure that the state in which it is set is always visited before the state in which it is referenced\.

  To ensure accuracy, the minimum time to which a timer should be set is 60 seconds\.

  Note that `'timeout()'` will return `TRUE` only the first time it is checked following the actual timer expiration and will return `FALSE` thereafter\.
+ `convert("type", "expression")`

  Returns the value of the expression converted to the specified type\. The type can be one of `String`, `Boolean`, or `Decimal` \(use one of these keywords or a string containing the keyword\)\. Only the following conversions will succeed and return a valid value: 
  + Boolean \-> String

    Returns the string "true" or "false"\.
  + Decimal \-> String
  + String \-> Boolean
  + String \-> Decimal

    The string specified must be valid representation of a decimal number, or `convert()` fails\.

  If `convert()` fails to return a valid value, the expression that it's a part of is also invalid\. This result is equivalent to `false` and won't trigger the `actions` or transition to the `nextState` specified as part of the event in which the expression occurs\.

**References:**
+ Inputs:

  `$input.input-name.path-to-datum`

  `"input-name"` is an input you have created using [ CreateInput](iotevents-commands.md#api-iotevents-CreateInput)\.

  For example, given an input named `"TemperatureInput"` for which you have defined `inputDefinition.attributes.jsonPath` entries that amount to the following available fields:

  ```
  {
      "temperature": 78.5,
      "date": 2018-10-03T16:09:09Z
  }
  ```

  To reference the value of `temperature`, use the following\.

  ```
  $input.TemperatureInput.temperature
  ```

  For fields whose values are arrays, members of the array can be referenced using `[n]`\. For example, given the following:

  ```
  {
      "temperatures": [
        78.4,
        77.9,
        78.8
      ],
      "date": 2018-10-03T16:09:09Z
  }
  ```

  The value `78.8` can be referenced with the following\.

  ```
  $input.TemperatureInput.temperatures[2]
  ```
+ Variables:

  `$variable.variable-name`

  `"variable-name"` is a variable you have defined using [ CreateDetectorModel](iotevents-commands.md#api-iotevents-CreateDetectorModel)\.

  For example, given a variable named `"TechnicianID"` that you have defined using `detectorDefinition.states.onInputEvents.actions.setVariable.variableName`, you can reference the \(string\) value most recently given to the variable with the following\.

  ```
  $variable.TechnicianID
  ```

  The values of variables can only be set using the `setVariable` action\. They can't be assigned a value in an expression\. Also note that a variable can't be "unset", that is, it can't be assigned the value `null`\.

**Note**  
In references that use identifiers that don't follow the \(regular expression\) pattern `[a-zA-Z][a-zA-Z0-9_]*`, you must enclose those identifiers in backticks \(```\)\. For example, a reference to an input `"MyInput"` with a field named `"_value"` must specify this field as `$input.MyInput.`_value``\.