# Pass<a name="amazon-states-language-pass-state"></a>

A `Pass` state \(`"Type": "Pass"`\) passes its input to its output, without performing work\. `Pass` states are useful when constructing and debugging state machines\.

In addition to the [common state fields](amazon-states-language-common-fields.md), `Pass` states allow the following fields\.

** `Result` \(Optional\)**  
Refers to the output of a virtual task that is passed on to the next state\. If you include the `ResultPath` field in your state machine definition, `Result` is placed as specified by `ResultPath` and passed on to the next state\.

** `ResultPath` \(Optional\)**  
Specifies where to place the *output* \(relative to the input\) of the virtual task specified in `Result`\. The input is further filtered as specified by the `OutputPath` field \(if present\) before being used as the state's output\. For more information, see [Input and Output Processing](concepts-input-output-filtering.md)\.

** `Parameters` \(Optional\)**  
Creates a collection of key\-value pairs that will be passed as input\. You can specify `Parameters` as a static value or select from the input using a path\. For more information, see [InputPath, Parameters and ResultSelector](input-output-inputpath-params.md)\.

## Pass State Example<a name="pass-state-example"></a>

Here is an example of a `Pass` state that injects some fixed data into the state machine, probably for testing purposes\.

```
"No-op": {
  "Type": "Pass",
  "Result": {
    "x-datum": 0.381018,
    "y-datum": 622.2269926397355
  },
  "ResultPath": "$.coords",
  "Next": "End"
}
```

Suppose the input to this state is the following\.

```
{
  "georefOf": "Home"
}
```

Then the output would be this\.

```
{
  "georefOf": "Home",
  "coords": {
    "x-datum": 0.381018,
    "y-datum": 622.2269926397355
  }
}
```