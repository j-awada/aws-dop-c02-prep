# Content Delivery Network

CloudFront is a content delivery network, used to improve the delivery of content to the viewers. It does so by caching and by using an efficient global network.

CloudFront is for download operations only, and uploads are done directly to the origin. CloudFront does read-only caching.

**Some terms:**

* Origin: the source location of the content. Can be S3 Origin or Custom Origin. Groups of origins can improve resiliency.

* Distribution: the unit of configuration within CloudFront.

* Behaviour: is contained within a distribution and works on the principal of pattern match.

* Edge location: pieces of the global infrastructure where data is cached locally. They are mostly used for storage/caching of data for CloudFront.

* Regional edge cache: they are bigger than edge locations and they are fewer. They are designed to cache data accessed less frequently but where there is a performance benefit.

## How it works?

1. Bob uploads some content to an S3 bucket which will be the origin of the CloudFront distribution.

2. Bob creates a CLoudFront distribution. The S3 bucket will be the origin in that distribution.

The distribution will have a unique domain name which ends in `.cloudfront.net` and/or an alternate domain name.

Bob can then deploy the distribution to the CloudFront network. The configuration will be pushed to edge locations which will cache the content distributed globally as close to the customers as possible.

In between the edge locations and the origin are the regional edge caches. These are bigger than the edge locations.

3. When customers attempt to access Bob's data, they will be directed towards the closest edge location. The following scenatrios can take place:

* cache hit: the data is on the edge location and is served fast with low latency.
* cache miss: the edge location missing the data will check the closest regional edge cache.
    - if the data is there it will be served.
    - if the data is not there, origin fetch will take place where the data is fetched from the source. The regional edge location pushes the data to the edge location that requested it where the data be be cached there and returned to the user.

## CloudFront Behaviours

The Behaviour can be configured from the Distribution. There is a default behaviour (*) which matches all patterns.

Behaviour configurations include the origin source, protocol and methods, viewer access, Lambda function association, caching and TTL controls.


## TTL and invalidation

When the data on the edge location becomes stale and needs to be refreshed.

The source could return a status 304 when the object is not expired and 200 status in case it needs to be refreshed along with the latest object.

An object is not expired when it is within its TTL (Time To Live). The default TTL is 24 hours.

Minimum TTL and Maximum TTL can be set using HTTP Headers sent from origin and they act as limiters.

**Cache invalidation**: invalidates an object independent of its TTL. It is performed on a distribution and uses pattern matching.

## CloudFront with other AWS services

CloudFront integrates with the AWS Certificate Manager (ACM) so SSL certificates can be used with CloudFront.

ACM can generate or import certificates. Generated certificates renew automatically but imported ones need to be renewed manually.

Publicly-trusted certificates and not self-signed are allowed to be used with CloudFront distributions.

Not all services within AWS are supported by ACM, in general it is CloudFront and ALBs NOT EC2 which is a self-managed service. Because with EC2, you can always access the certificate which is not allowed by ACM.

ACM is a regional service. Certificates are locked to the region they are generated in.

For global services like CloudFront, `us-east-1` is always used for ACM certificates.

There exists 2 SSL connections:

* Viewer protocol: Viewer <=> CloudFront
* Origin protocol: CloudFront <=> Origin

Both need valid public certificates.

If the origin is S3, S3 handles certificates natively and nothing needs to be done.

## CloudFront and SNI

In the past, every SSL-enabled website needed its own IP address. It is however possible to host 2 domains on the same server. The latter is achieved using a header in the request to indicate which website is requested.

In 2023, SNI (Serve Name Indication) appeared which is a TLS extention that allows a client to tell the server which domain it is trying to access. This occurs within the TLS handshake before HTTP. This allows 1 server to host several websites. The issue is that older browser do not support SNI. Hence why CLoudFront supports both SNI but also IP per server at an extra cost.

## Origin types

Origins are where CloudFront goes to get content Origins are selected from the Behaviour section.

Origins can be:

* S3 buckets
* AWS media package channel endpoints
* AWS media store container endpoints
* everything else i.e. web servers

With S3 as origin, the same protocol is used on the viewer and origin sides.

## Cache invalidation

From the AWS console and in a distribution, under the invalidations tag, you can create a new invalidation. An invalidation is an operation which sends a directive to every edge location to invalidate 1 or more objects.
