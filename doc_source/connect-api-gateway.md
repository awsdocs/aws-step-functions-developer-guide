# Call API Gateway with Step Functions<a name="connect-api-gateway"></a>

Step Functions can control certain AWS services directly from the Amazon States Language\. For more information, see the following:
+ [Service Integrations](concepts-service-integrations.md)
+ [Pass Parameters to a Service API](connect-parameters.md)

You use Amazon API Gateway to create, publish, maintain, and monitor HTTP and REST APIs\. To integrate with API Gateway, you define a `Task` state in Step Functions that directly calls an API Gateway HTTP or API Gateway REST endpoint, without writing code or relying on other infrastructure\. A `Task` state definition includes all the necessary information for the API call\. You can also select different authorization methods\.

**Note**  
Step Functions supports the ability to call HTTP endpoints through API Gateway, but does not currently support the ability to call generic HTTP endpoints\.

## API Gateway feature support<a name="connect-api-gateway-support"></a>

The Step Functions API Gateway integration supports some, but not all API Gateway features\. For a more detailed list of supported features, see the following\. 
+ Supported by both the Step Functions API Gateway REST API and API Gateway HTTP API integrations:
  + **Authorizers**: IAM \(using [Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/sigv4_signing.html)\), No Auth, Lambda Authorizers \(request\-parameter based and token\-based with custom header\)
  + **API types**: Regional
  + **API management**: API Gateway API domain names, API stage, Path, Query Parameters, Request Body
+ Supported by the Step Functions API Gateway HTTP API integration but not the Step Functions API Gateway REST API integration:
  + Edge\-optimized APIs
+ Unsupported by the Step Functions API Gateway integration:
  +  **Authorizers**: Amazon Cognito, Native Open ID Connect / OAuth 2\.0, Authorization header for token\-based Lambda authorizers 
  +  **API types**: Private 
  +  **API management**: Custom domain names 

For more information about API Gateway and its HTTP and REST APIs, see the following\.
+  The [Amazon API Gateway concepts](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-basic-concept.html) page\. 
+  [Choosing between HTTP APIs and REST APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html) in the API Gateway developer guide\. 

## Request format<a name="connect-api-gateway-requests"></a>

When you create your `Task` state definition, Step Functions validates the parameters, builds the necessary URL to perform the call, then calls the API\. The response includes the HTTP status code, headers and response body\. The request format has both required and optional parameters\.

### Required request parameters<a name="connect-api-gateway-requests-required"></a>
+ `ApiEndpoint`
  + Type: `String`
  + The hostname of an API Gateway URL\. The format is `<API ID>.execute-api.<region>.amazonaws.com`\.

    The API ID can only contain a combination of the following alphanumeric characters: `0123456789abcdefghijklmnopqrstuvwxyz`
+ `Method`
  + Type: `Enum`
  + The HTTP method, which must be one of the following: 
    + `GET`
    + `POST`
    + `PUT`
    + `DELETE`
    + `PATCH`
    + `HEAD`
    + `OPTIONS`

### Optional request parameters<a name="connect-api-gateway-requests-optional"></a>
+ `Headers`
  + Type: `JSON`
  + HTTP headers allow a list of values associated with the same key\.
+ `Stage`
  + Type: `String`
  + The name of the stage where the API is deployed to in API Gateway\. It's optional for any HTTP API that uses the `$default` stage\. 
+ `Path`
  + Type: `String`
  + Path parameters that are appended after the API endpoint\. 
+ `QueryParameters`
  + Type: `JSON`
  + Query strings allow a list of values associated with the same key\. 
+ `RequestBody`
  + Type: `JSON` or `String`
  + The HTTP Request body\. Its type can be either a `JSON` object or `String`\. 
+ `AuthType`
  + Type: `JSON`
  + The authentication method\. The default method is `NO_AUTH`\. The allowed values are: 
    + `NO_AUTH`
    + `IAM_ROLE`
    + `RESOURCE_POLICY`

    See **Authentication and authorization** for more information\.

**Note**  
For security considerations, the following HTTP header keys are not currently permitted:  
Anything prefixed with `X-Forwarded`, `X-Amz` or `X-Amzn`\.
`Authorization`
`Connection`
`Content-md5`
`Expect`
`Host`
`Max-Forwards`
`Proxy-Authenticate`
`Server`
`TE`
`Transfer-Encoding`
`Trailer`
`Upgrade`
`Via`
`Www-Authenticate`

The following code example shows how to invoke API Gateway using Step Functions\.

```
{
    "Type": "Task", 
    "Resource":"arn:aws:states:::apigateway:invoke", 
    "Parameters": {
        "ApiEndpoint": "example.execute-api.us-east-1.amazonaws.com",
        "Method": "GET", 
        "Headers": { 
            "key": ["value1", "value2"] 
        },
        "Stage": "prod",
        "Path": "bills",
        "QueryParameters": {
            "billId": ["123456"]
        },
        "RequestBody": {},
        "AuthType": "NO_AUTH"
    } 
}
```

## Authentication and authorization<a name="connect-api-gateway-auth"></a>

You can use the following authentication methods:
+ **No authorization**: Call the API directly with no authorization method\.
+ **IAM role**: With this method, Step Functions assumes the role of the state machine, signs the request with [Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/sigv4_signing.html) \(SigV4\), then calls the API\.
+ **Resource policy**: Step Functions authenticates the request, and then calls the API\. You must attach a resource policy to the API which specifies the following:

  1. The state machine that will invoke API Gateway\.
**Important**  
You must specify your state machine to limit access to it\. If you do not, then any state machine that authenticates its API Gateway request with **Resource policy** authentication to your API will be granted access\.

  1. That Step Functions is the service calling API Gateway: `"Service": "states.amazonaws.com"`\.

  1. The resource you want to access, including:
     + The *region*\.
     + The *account\-id* in the specified region\.
     + The *api\-id*\.
     + The *stage\-name*\.
     + The *HTTP\-VERB* \(method\)\.
     + The *resource\-path\-specifier*\.

  For an example resource policy, see [IAM policies for Step Functions and API Gateway](api-gateway-iam.md)\. 

  For more information on the resource format, see [Resource format of permissions for executing API in API Gateway ](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-control-access-using-iam-policies-to-invoke-api.html#api-gateway-iam-policy-resource-format-for-executing-api) in the API Gateway Developer Guide\.
**Note**  
Resource policies are only supported for the REST API\.

## Service integration patterns<a name="connect-api-gateway-patterns"></a>

The API Gateway integration supports two service integration patterns: 
+ [Request Response](connect-to-resource.md#connect-default), which is the default integration pattern\. It lets Step Functions progress to the next step immediately after it receives an HTTP response\.
+ [Wait for a Callback with the Task Token](connect-to-resource.md#connect-wait-token) \(`.waitForTaskToken`\), which waits until a task token is returned with a payload\. To use the `.waitForTaskToken` pattern, append \.waitForTaskToken to the end of the **Resource** field of your task definition as shown in the following example: 

  ```
  {
      "Type": "Task", 
      "Resource":"arn:aws:states:::apigateway:invoke.waitForTaskToken", 
      "Parameters": {
          "ApiEndpoint": "example.execute-api.us-east-1.amazonaws.com",
          "Method": "POST", 
          "Headers": { 
              "TaskToken.$": "States.Array($$.Task.Token)"
          },
          "Stage": "prod",
          "Path": "bills/add",
          "QueryParameters": {},
          "RequestBody": {
              "billId": "my-new-bill"
          },
          "AuthType": "IAM_AUTH"
      } 
  }
  ```

## Output format<a name="connect-api-gateway-output"></a>

The following output parameters are provided:


| Name | Type | Description | 
| --- | --- | --- | 
| ResponseBody | JSON or String | The response body of the API call\. | 
| Headers | JSON | The response headers\.  | 
| StatusCode | Integer | The HTTP status code of the response\. | 
| StatusText | String | The status text of the response\. | 

An example response:

```
{
    "ResponseBody": {
        "myBills": []
    },
    "Headers": { 
        "key": ["value1", "value2"]
    }, 
    "StatusCode": 200,
    "StatusText": "OK" 
}
```

## Error handling<a name="connect-api-gateway-errors"></a>

When an error occurs, an `error` and `cause` is returned as follows: 
+ If the HTTP status code is available, then the error will be returned in the format `APIGateway.<HTTP Status Code>`\.
+ If the HTTP status code is not available, then the error will be returned in the format `APIGateway.<Exception>`\.

In both cases, the `cause` is returned as a string\.

The following example shows a response where an error has occurred:

```
{
    "error": "APIGateway.403", 
    "cause": "{\"message\":\"Missing Authentication Token\"}"
}
```

**Note**  
A status code of `2XX` indicates success, and no error will be returned\. All other status codes or thrown exceptions will result in an error\.

For more information see:
+  [Amazon API Gateway concepts](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-basic-concept.html) in the API Gateway Developer Guide\.
+ IAM policies for Step Functions and [Amazon API Gateway](api-gateway-iam.md)
+ A sample project that shows how to [Make a call to API Gateway](sample-apigateway-workflow.md)

 [Amazon API Gateway concepts](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-basic-concept.html) in the API Gateway Developer Guide\.