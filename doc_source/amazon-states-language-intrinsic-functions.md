# Intrinsic functions<a name="amazon-states-language-intrinsic-functions"></a>

The Amazon States Language provides several Intrinsics to allow basic operations without `Task` states\. Intrinsics are constructs that look like functions in programming languages and can be used to help Payload Builders process the data going to and from Task Resources\.

**Note**  
To indicate the use of intrinsic functions, you must specify `.$` in the key value in your state machine definitions, as shown in the following example:  

```
"KeyId.$": "States.Array($.Id)"
```

** `States.Format`**  
 Supports string construction from literal and interpolated values, and takes one or more arguments\. The Value of the first argument must be a string, and may include zero or more instances of the character sequence `{}`\. There must be as many remaining arguments in the Invocation as there are occurrences of `{}`\. The interpreter returns the first\-argument string with each `{}` replaced by the Value of the positionally\-corresponding argument in the Intrinsic invocation\.  
 Example: given the following input:   

```
{
 "name": "Arnav",
 "template": "Hello, my name is {}."
}
```
States\.Format can be used as follows:  

```
States.Format('Hello, my name is {}.', $.name)
```
or  

```
States.Format($.template, $.name)
```
Both result in an output of `Hello, my name is Arnav.`

** `States.StringToJson` **  
Takes a single argument, a reference path to an escaped JSON string\.  
The interpreter applies a JSON parser returns its parsed JSON form\. For example, given the following input:  

```
{
 "escapedJsonString": "{\"foo\": \"bar\"}"
}
```
You could use  

```
States.StringToJson($.escapedJsonString)
```
Which would return `{ "foo": "bar" }`

** `States.JsonToString` **  
Takes a single argument\. This argument must be a Path\. The interpreter returns a string which is a JSON text representing the data identified by the Path\. For example, given the following input:  

```
{
  "unescapedJson": {
     "foo": "bar"
  }
}
```
You could use  

```
States.JsonToString($.unescapedJson)
```
Which would return `{\"foo\": \"bar\"}`

** `States.Array`**  
This intrinsic takes zero or more arguments\. The interpreter returns a JSON array containing the Values of the arguments, in the order provided\. For example, given the following input:  
For example, if you use the following payload:  

```
{
  "Parameters": {
    "foo.$": "States.Array('Foo', 2022, $.someJson, null)"
  }
}
```
And provide the following input for the above payload:  

```
{
  "someJson": {
    "random": "abcdefg"
  },
  "zebra": "stripe"
}
```
The new payload after processing is:  

```
{
  "foo": [
    "Foo",
    2022,
    {
      "random": "abcdefg"
    },
    null
  ]
}
```

## Reserved characters in intrinsic functions<a name="amazon-states-language-intrinsic-functions-escapes"></a>

 The following characters are reserved for intrinsic functions, and must be escaped with a backslash \('\\'\) if you want them to appear in the Value: `'``{``}`, and `\`\. 

If the character `\` needs to appear as part of the value without serving as an escape character, it must first be escaped with a backslash\. Reserved characters for intrinsic invocations are escaped as follows:
+ The literal string `\'` represents `'`\.
+ The literal string `\{` represents `{`\.
+ The literal string `\}` represents `}`\.
+ The literal string `\\` represents `\`\.

In JSON, backslashes contained in a string literal value must be escaped with another backslash\. The equivalent list for JSON is:
+ The escaped string `\\'` represents `'`\.
+ The escaped string `\\{` represents `{`\.
+ The escaped string `\\}` represents `}`\.
+ The escaped string `\\\\` represents `\`\.

**Note**  
If an open escape backslash `\` is found in the intrinsic invocation string, the interpreter will throw a runtime error\.

## Fields that support intrinsic functions<a name="amazon-states-language-intrinsic-functions-states"></a>

The following table shows which fields support intrinsic functions for each state\.


**Fields that support intrinsic functions**  

| State | 
| --- |
|  | **Pass** | **Task** | **Choice** | **Wait** | **Succeed** | **Fail** | **Parallel** | **Map** | 
| --- |--- |--- |--- |--- |--- |--- |--- |--- |
| InputPath |  |  |  |  |  |  |  |  | 
| Parameters | ✓ | ✓ |  |  |  |  | ✓ | ✓ | 
| ResultSelector |  | ✓ |  |  |  |  | ✓ | ✓ | 
| ResultPath |  |  |  |  |  |  |  |  | 
| OutputPath |  |  |  |  |  |  |  |  | 
| Variable |  |  |  |  |  |  |  |  | 
| <Comparison Operator>Path |  |  |  |  |  |  |  |  | 
| TimeoutSecondsPath |  |  |  |  |  |  |  |  | 
| HeartbeatSecondsPath |  |  |  |  |  |  |  |  | 