# Expression usage<a name="expression-usage"></a>

You can specify values in a detector model in the following ways:
+ Enter supported expressions in the AWS IoT Events console\.
+ Pass the expressions to the AWS IoT Events APIs as parameters\.

Expressions support literals, operators, functions, references, and substitution templates\.

**Important**  
Your expressions must reference a integer, decimal, string, or Boolean value\.

## Writing AWS IoT Events expressions<a name="write-expressions"></a>

See the following examples to help you write your AWS IoT Events expressions:

**Literal**  
For literal values, the expressions must contain single quotes\. A Boolean value must be either `true` or `false`\.  

```
'123'        # Integer
'123.12'     # Decimal
'hello'      # String
'true'       # Boolean
```

**Reference**  
For references, you must specify either variables or input values\.  
+ The following input references a decimal number, `10.01`\.

  ```
  $input.GreenhouseInput.temperature
  ```
+ The following variable references a string, `Greenhouse Temperature Table`\.

  ```
  $variable.TableName
  ```

**Substitution template**  
For a substitution template, you must use `${}`, and the template must be in single quotes\. A substitution template can also contain a combination of literals, operators, functions, references, and substitution templates\.  
+ The evaluated result of the following expression is a string, `50.018 in Fahrenheit`\.

  ```
  '${$input.GreenhouseInput.temperature * 9 / 5 + 32} in Fahrenheit'
  ```
+ The evaluated result of the following expression is a string, `{\"sensor_id\":\"Sensor_1\",\"temperature\":\"50.018\"}`\.

  ```
  '{\"sensor_id\":\"${$input.GreenhouseInput.sensors[0].sensor1}\",\"temperature\":\"${$input.GreenhouseInput.temperature*9/5+32}\"}'
  ```

**String concatenation**  
For a string concatenation, you must use `+`\. A string concatenation can also contain a combination of literals, operators, functions, references, and substitution templates\.  
+ The evaluated result of the following expression is a string, `Greenhouse Temperature Table 2000-01-01`\.

  ```
  'Greenhouse Temperature Table ' + $input.GreenhouseInput.date
  ```