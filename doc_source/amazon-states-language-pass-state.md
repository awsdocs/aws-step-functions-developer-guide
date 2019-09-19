# Pass<a name="amazon-states-language-pass-state"></a>

A `Pass` state \(`"Type": "Pass"`\) passes its input to its output, without performing work\. `Pass` states are useful when constructing and debugging state machines\.

In addition to the [common state fields](amazon-states-language-common-fields.md), `Pass` states allow the following fields\.

** `Result` \(Optional\)**  
Treated as the output of a virtual task to be passed to the next state, and filtered as specified by the `ResultPath` field \(if present\)\.

** `ResultPath` \(Optional\)**  
Specifies where \(in the input\) to place the "output" of the virtual task specified in `Result`\. The input is further filtered as specified by the `OutputPath` field \(if present\) before being used as the state's output\. For more information, see [Input and Output Processing](concepts-input-output-filtering.md)\.

** `Parameters` \(Optional\)**  
Create a collection of key\-value pairs that will be passed as input\. Values can be static, or selected from the input with a path\. For more information, see [InputPath and Parameters](input-output-inputpath-params.md)\.

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

Suppose the input to this state is this:

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