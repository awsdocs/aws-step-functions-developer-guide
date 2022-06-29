# Data processing<a name="use-cases-data-processing"></a>

As the volume of data grows, coming from increasingly diverse sources, organizations find they need to move quickly to process this data to ensure they make faster, well\-informed business decisions\. To process data at scale, organizations need to elastically provision resources to manage the information they receive from mobile devices, applications, satellites, marketing and sales, operational data stores, infrastructure, and more\.

Step Functions provides the scalability, reliability, and availability needed to successfully manage your data processing workflows\. You can manage millions of concurrent executions with Step Functions as it scales horizontally and provides fault\-tolerant workflows\. Process data faster using parallel executions like Step Functions’ `Parallel` state type, or dynamic parallelism using its `Map` state type\. As part of your workflow, you can use the `Map` state to iterate over objects in a static data store like an Amazon S3 bucket\. Step Functions also lets you easily retry failed executions, or choose a specific way to handle errors without the need to manage a complex process\.

Depending upon your data processing needs, Step Functions directly integrates with other data processing services provided by AWS such as [AWS Batch](connect-batch.md) for batch processing, [Amazon EMR](connect-emr.md) for big data processing, [AWS Glue](connect-glue.md) for data preparation, [Athena](connect-athena.md) for data analysis, and [AWS Lambda](connect-lambda.md) for compute\. 

Examples of the types of data processing workflows that customers use Step Functions to accomplish include:

**File, video, and image processing**
+ Take a collection of video files and convert them to other sizes or resolutions that are ideal for the device they will be displayed on, such as mobile phones, laptops, or a television\. 
+ Take a large collection of photos uploaded by users and convert them into thumbnails or various resolution images that can then be displayed on users’ websites\. 
+ Take semi\-structured data, such as a CSV file, and combine it with unstructured data, such as an invoice, to produce a business report that is sent to business stakeholders monthly\.
+ Take earth observing data collected from satellites, convert it into formats that align with each other and then add other data sources collected on earth for additional insight\. 
+ Take the transportation logs from various modes of transportation for products and look for optimizations using Monte Carlo Simulations and then send reports back to the organizations and people that are relying on you to ship their goods\.

**Coordinate extract, transform and load \(ETL\) jobs:**
+ Combine sales opportunity records with marketing metric datasets through a series of data preparation steps using AWS Glue, and produce business intelligence reports that can be used across the organization\. 
+ Create, start, and terminate an Amazon EMR cluster for big data processing\. 

**Batch processing and High Performance Computing \(HPC\) workloads:**
+ Build a genomics secondary analysis pipeline that processes raw whole genome sequences into variant calls\. Align raw files to a reference sequence, and call variants on a specified list of chromosomes using dynamic parallelism\. 
+ Find efficiencies in the production of your next mobile device or other electronics by simulating various layouts using different electric and chemical compounds\. Run large batch processing of your workloads through various simulations to get the optimal design\.