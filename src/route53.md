# Route53

Is AWS's managed DNS product which provides 2 main services:

* Domain registration
* Hosting zone files on managed nameservers

It is a global service with a single database i.e. no region is needed on usage. It is globally resilient.

## How it works

Route53 has relationships with all of the domain registries eg. `.com`, `.io`, `.net`. And relationships with the PIR organisation for the `.org` registery.

First Route53 checks with the registry for the top-level domain if the domain is available.

Creates a zone file for the domain being registered. And allocates nameservers for this zone, generally 4 nameservers per zone. They are distributed globally.

Zones are called hosted zones in AWS terminology. AWS puts the zone file on the 4 nameservers.

It communicates with the said registry and adds the nameserver (NS) records into the zone file for the `.org` top-level domain using NS records on the `.org` registry. Which then indicates that the 4 nameservers are authoritative for the domain.

## Hosted zones

Zone files are hosted on AWS managed nameservers. A hosted zone on AWS can be public or private.

## DNS record types

**A and AAAA**

Given a DNS zone eg. `google.com`, these records map IP addresses to DNS names.

A records map to IP v4 address.

AAAA records map to IP v6 address.

**CNAME**

Stands for canonical name. Given a zone, the CNAME allows you to create DNS shortcuts or host to host records. It does not point to an IP.

For example, CNAME ftp, CNAME mail and CNAME www all point to an A server record which means they will all resolve to the same IP v4 address.

**TXT**

These records allow you to add arbitrary text to a domain. For example to prove domain ownership.

**MX**

It is how a server can find the mail server for a specific domain to connect to via SMTP. It does that via an MX lookup or query.

MX records have 2 parts: priority and value.

The value can be a host eg. `mail` which resolves to `mail.mydomain.com` i.e. same zone.

The value can have a dot at the end `mail.other.domain.` which means it is a fully qualified domain name i.e. can point to the same zone or something outside.

**NS**

Allows delegation to occur in DNS. NS records in the organisation registry eg. `.com` point at NS servers managed by AWS that host the zone.

## TTL (Time to Live)

Can be set on DNS records (in seconds). For example TLS of 3600 means that the result of a DNS query is stored/cached at the resolver server for 3600 seconds.

Below is a diagram of walking the tree to get a result from the authoritative source.

<div align="center">
    <img src="./images/ttl.png" alt="cloud services" width="500" />
</div>

## R53 Public Hosted Zones

A hosted zone is a DNS database for a given section of the global DNS database for a domain.

They are created automatically (or manually) when you register a domain.

There is a monthly fee to host a hosted zone and a fee for queries made against the hosted zone.

A zone hosts DNS records.

A public hosted zone is hosted on public nameservers and can be accessible from the public internet and VPCs.

## R53 Private Hosted Zones

It is not public and is associated with VPCs in AWS. It is inaccessible from public internet.

Split-view is when you have public and private hosted zones. You can expose certain records to the public internet and keep some records private for internal use.

<div align="center">
    <img src="./images/private_hosted_zone.png" alt="cloud services" width="500" />
</div>

## CNAME vs. ALIAS

An A record maps a name to a v4 IP address. A CNAME maps a name to another name eg. www.mydomain.io => mydomain.io

CNAME can not use a naked domain (apex domain) eg. mydomain.io to point at another name.

An ALIAS record is implemented by AWS and maps a name to an AWS resource. ALIAS can use apex domains.

## R53 health checks

Health checks are configured separately and are used by records. They can be TCP, HTTP/HTTPS and HTTP/HTTPS with String Matching which is a check where a string must appear in the first 5120 bytes of the response body.

The health check can be created from the Route53 dashboard.

If more than 18% of the health checkers report as healthy then the health check passes.

## Failover routing

Can add a primary record and a secondary with the same name pointing at different resources. For example when an A record points at an EC2 instance whose health check fails, the secondary record can point at an S3 bucket serving a static page for when the main application is down.

Traffic is routed to a resource when the resource is healthy and when it is not, traffic is pointed to another resource.

## Multi value routing

It is used when you have multiple sources which can all service requests and you want them all health checked and returned at random.

Create multiple values of the same record eg.

A => IP1

A => IP2

A => IP3

Each record has a health check.

This is not a substitute to load balancers.

## Weighted routing

Used as a simple form of load balancing, when you want to test new versions of software.

For example, 3 A records that point at different IP addresses (EC2 instances). Each record has a weight (%) associated to it.

It does not have to be in %, the weight is calculated based on the total weight.

## Latency-based routing

When you are trying to optimise for performance and user experience.

For example, 3 A records pointing at different IP addresses with each record having a different AWS region.

The idea is that you are specifying the region for where the infrastructure of that record is created.

Route53 will route the user to the region closer to them.

## Geolocation routing

Similar to latency-based routing only the location is used to tag the records.

Geolocation does not return the closest record only the relevant record.

I.e. match a client location with a custom record.

A default record can be returned when no record matches a user.

Ideal for when you want to restrict content by location. Eg. only people located in the US receive the record.

## Geoproximity routing

Provides records as close to the customer as possible by calculating the distance between the record and the customer.

Unlike latency routing, it works by distance but also allows you to place bias on locations.

You define a bias if you choose to route more resources to a location.

## R53 interoperability

When you register a domain with Route53, it does 2 jobs:

* Domain registrar (domain registration fee)
* Domain hosting (allocating 4 nameservers)

Route53 also communicates with the registry of the top level domain (TLD); eg. the `.com` registry, to add a new entry for the domain containing the 4 nameservers.

When you register a domain outside of Route53, it does 1 of the 2 jobs.
