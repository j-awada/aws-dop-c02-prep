# Content Delivery Network

CloudFront is a content delivery network, used to improve the delivery of content to the viewers. It does so by caching and by using an efficient global network.

**Some terms:**

* Origin: the source location of the content. Can be S3 Origin or Custom Origin.

* Distribution: the unit of configuration within CloudFront.

* Behaviour: is contained within a distribution and works on the principal of pattern match.

* Edge location: pieces of the global infrastructure where data is cached locally. They are mostly used for storage/caching of data for CloudFront.

* Regional edge cache: they are bigger than edge locations and they are fewer. They are designed to cache data accessed less frequently but where there is a performance benefit.

## How it works?

1. Bob uploads some content to an S3 bucket which will be the origin of the CloudFront distribution.

2. Bob creates a CLoudFront distribution. The S3 bucket will be the origin in that distribution.

The distribution will have a unique domain name which ends in `.cloudfront.net`.

Bob can then deploy the distribution to the CloudFront network. The configuration will be pushed to edge locations which will cache the content distributed globally as close to the customers as possible.

In between the edge locations and the origin are the regional edge caches. These are bigger than the edge locations.

3. When customers attempt to access Bob's data, they will be directed towards the closest edge location. The following scenatrios can take place:

* cache hit: the data is on the edge location and is served fast with low latency.
* cache miss: the edge location missing the data will check the closest regional edge cache.
    - if the data is there it will be served.
    - if the data is not there, origin fetch will take place where the data is fetched from the source. The regional edge location pushes the data to the edge location that requested it where the data be be cached there and returned to the user.

## CloudFront with other AWS services

CloudFront integrates with the AWS Certificate Manager (ACM) so SSL certificates can be used with CloudFront.

CloudFront is for download operations only, and uploads are done directly to the origin. CloudFront does read-only caching.
