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
