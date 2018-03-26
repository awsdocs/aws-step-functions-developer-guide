# Use ARNs Instead of Passing Large Payloads<a name="avoid-exec-failures"></a>

Executions that pass large payloads of data between states can be terminated\. If the data you are passing between states might grow to over 32 K, use Amazon Simple Storage Service \(Amazon S3\) to store the data, and pass the Amazon Resource Name instead of the raw data\. Alternatively, adjust your implementation so that you pass smaller payloads in your executions\.

For more information, see:
+ [Amazon Simple Storage Service Developer Guide](http://docs.aws.amazon.com/AmazonS3/latest/dev/)
+ [Amazon Resource Names \(ARNs\)](http://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)