# S3 misc.

## S3 security

S3 is private by default.

An S3 bucket policy is a form of resource policy; it is like identity policies but attached to a bucket.

Bucket policies can be complex for eg. deny access to an IP address range / if the identity using the bucket does not use MFA.

**Access Control Lists (ACLs)**

Are legacy and not recommended to use by AWS but they are used to apply security to objects or buckets.
