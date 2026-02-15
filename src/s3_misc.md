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
