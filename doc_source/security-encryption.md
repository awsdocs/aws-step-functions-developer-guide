# Encryption in AWS Step Functions<a name="security-encryption"></a>

## Encryption at Rest<a name="security-encryption-at-rest"></a>



Step Functions always encrypts your data at rest\. Data in AWS Step Functions is encrypted at rest using transparent server\-side encryption\. This helps reduce the operational burden and complexity involved in protecting sensitive data\. With encryption at rest, you can build security\-sensitive applications that meet encryption compliance and regulatory requirements

## Encryption in transit<a name="security-encryption-in-transit"></a>

Step Functions encrypts data in transit between the service and other integrated AWS services \(see [Using AWS Step Functions with other services](concepts-service-integrations.md)\)\. All data that passes between Step Functions and integrated services is encrypted using Transport Layer Security \(TLS\)\.