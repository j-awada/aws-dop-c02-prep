# S3 misc.

## S3 security

S3 is private by default.

An S3 bucket policy is a form of resource policy; it is like identity policies but attached to a bucket.

Bucket policies can be complex for eg. deny access to an IP address range / if the identity using the bucket does not use MFA.

**Access Control Lists (ACLs)**

Are legacy and not recommended to use by AWS but they are used to apply security to objects or buckets.

## S3 static website hosting

It allows access via standard HTTP by individuals using a web browser.

When you enable this, you have to set an index and an error document.

## S3 bucket object versioning

By default, this is disabled. Once enabled, it can not be disabled again. The bucket however can be suspended and the versions deleted then re-enabled.

Versioning lets you store multiple versions of an object in a bucket.

An object has a key (name) and an ID (used for versioning). When versioning is disabled, the ID is null. But when versioning is enabled and an object is accessed without specifying an ID, the latest object is assumed.

When an object is deleted without specifying a version, a Delete Marker is added which is a special version of an object which hides all previous versions of that object. The Delete Marker can be deleted and the object can be active again.

To fully delete an object, you need to specify a version.

## S3 bucket upload

By default, data is uploaded to S3 in a single blob of data in a **single stream**. A file is uploaded using the `s3:PutObject` action and put into a bucket.

But if a stream fails, the upload fails and nothing is uploaded. And a PUT upload is limited to 5GB of data in a single stream.

**Multipart upload** however improves the speed and reliability of uploads to S3. It does this by breaking data down into various blobs. The minimum size of the data uploaded should be 100MB.

An upload can be split into a max of 10,000 parts and each part can range in size from 5MB to 5GB. Unlike single stream upload, each part can fail in isolation and restart in isolation. Transfer rate; which is the speed of all parts, is therefore improved.

## S3 Accelerated Transfer

Data can take inefficient routes to reach its destination especially if travelling from country to another. This can slow down data transfer due to this low performance.

Transfer acceleration for S3 uses the network of AWS edge locations located globally in convenient locations. This is by default switched off and needs to be enabled given certain restrictions.

The AWS network is purposely build to connect regions with one another.

## S3 server-side encryption

Buckets are not encrypted, objects are. Objects can use different encryption settings.

Knowing that data to and from S3 is encrypted in transit, encryption at rest i.e. how data is stored on disk can be:

**client-side encryption:**

Data is encrypted at the client side and S3 receives data that is already encrypted. The client is responsible for the encryption keys and for the encryption process. Here, S3 is just used for storage.

**server-side encryption:**

Data is not encrypted on the client side but when it reaches S3 it gets encrypted. FYI encryption at rest is mandatory on S3.

Serve-side encryption for S3 objects has 3 types:

1. SSE-C (server-side encryption with customer-provided keys)

The customer is responsible for the keys but S3 handles the cryptographic operation.

2. SSE-S3 (server-side encryption with Amazon S3-managed keys)

This is the default. S3 provides the key and the encryption process.

3. SSE-KMS (server-side encryption with KMS keys stored in AWS KMS)

Here, the KMS service is involved. The client has control over the KMS key. S3 handles the encryption process.

## S3 bucket keys

This helps S3 scale and reduce cost when using KMS encryption.

Usually every object requires a unique KMS key to get encrypted. Which means that for every S3 object, a unique KMS key is needed. This increases cost due to the many requests to KMS, it might also be subject to throttling restrictions.

With bucket keys, instead of the KMS key being used to generate many data encryption keys, it is used to generate a time-limited bucket key. The bucket key is then used to generate data encryption keys for objects in the bucket. This reduces KMS API calls, reduces cost and improves scalability.
