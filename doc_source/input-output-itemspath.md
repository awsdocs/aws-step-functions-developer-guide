# ItemsPath<a name="input-output-itemspath"></a>

The `ItemsPath` field is used in a [`Map` state](amazon-states-language-map-state.md) to select an array in the input\. A `Map` state is used to iterate steps for each item in an array contained in the input\. By default, a `Map` state sets `ItemsPath` to `"$"` selecting the entire input\. If the input to the `Map` state is a JSON array it will run an iteration for each item in the array, passing that item to the iteration as input\. For an example of this, see the [Map State Tutorial](tutorial-creating-map-state-machine.md)\.

The `ItemsPath` field allows you to specify a location in the input to find the JSON array to use for iterations\. The value of `ItemsPath` must be a [Reference Path](amazon-states-language-paths.md#amazon-states-language-reference-paths), and it must identify a value that is a JSON array\. For instance, consider input to a `Map` state that includes two arrays, like the following example\.

```
{
  "ThingsPiratesSay": [
    {
      "say": "Avast!"
    },
    {
      "say": "Yar!"
    },
    {
      "say": "Walk the Plank!"
    }
  ],
  "ThingsGiantsSay": [
    {
      "say": "Fee!"
    },
    {
      "say": "Fi!"
    },
    {
      "say": "Fo!"
    },
    {
      "say": "Fum!"
    }
  ]
}
```

In this case, you could specify which array to use for `Map` state iterations by selecting a specific array with `ItemsPath`\. The following state machine definition specifies the `"ThingsPiratesSay"` array in the input using `ItemsPath`, and will run an iteration of the `SayWord` pass state for each item in the `"ThingsPiratesSay"` array\.

```
{
  "StartAt": "PiratesSay",
  "States": {
    "PiratesSay": {
      "Type": "Map",
      "ItemsPath": "$.ThingsPiratesSay",
      "Iterator": {
         "StartAt": "SayWord",
         "States": {
           "SayWord": {
             "Type": "Pass",
             "End": true
           }
         }
      },
      "End": true
    }
  }
}
```

When processing input, `ItemsPath` is applied after [`InputPath`](input-output-inputpath-params.md#input-output-inputpath)\. It operates on the effective input to the state, after `InputPath` has filtered the input\.

For more information on `Map` states, see the following\.
+  [Map State](amazon-states-language-map-state.md) 
+ [](amazon-states-language-map-state.md)
+ [Map State Example](amazon-states-language-map-state.md#map-state-examples)
+ [Use a Map State to Call Lambda Multiple Times](tutorial-creating-map-state-machine.md)
+ [Map State Input and Output Processing](amazon-states-language-map-state.md#amazon-states-language-map-state-output)
+ [Dynamically Process Data with a Map State](sample-map-state.md)