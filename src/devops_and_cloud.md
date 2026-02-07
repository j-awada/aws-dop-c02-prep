Here is some general information relevant to DevOps and cloud computing.

### Cloud computing

[NIST](https://nist.gov) defines 5 characteristics of cloud computing:

1. on-demand self-service
2. broad network access
3. resource pooling
4. rapid elasticity
5. measured service

**Cloud computing services**:

<img src="./images/cloud_computing.png" alt="cloud services" width="500" />

### DevOps principals

* Collaboration
* Communication
* Transparency

### Continuous integration

When developers regularly merge their code changes into a central repository, after which automated builds and tests run.

### Continuous delivery

When code changes are automatically built, tested and prepared for a release to production.

### Infrastructure as code (IaC)

When infrastructure is provisioned and managed using code and software development techniques such as version control and continuous integration.

### Monitoring and logging

Enabled organisations to see how applications and infrastructure performance impact the experience of the end user.

### Deployment strategies

Defines how software is delivered.

**In-place deployment**

* The previous version of software is stopped, the latest version is installed and the new version of the application is started
* New infrastructure does not have to be created
* Service outage is assumed

**Blue/green deployment**

* A technique for releasing an application by shifting traffic between 2 identical environments running differing versions of an application
* Enables launching a new version (green) of an application alongside an old version (blue) to monitor and test before traffic is re-routed
* minimises downtime and allows rollback

**Canary deployment**

Incrementally deploying a new version in a slow fashion until eventually replacing the old with the new version entirely.

**Linear deployment**

Traffic is shifted in equal increments in a way where you can specify the percentage of traffic to be shifted every number of minutes.

**All-at-once deployment**

All traffic is shifted from original environmnet to the new one all at once.

### Forward and reverse proxy

**Forward proxy**

A server that sits infront of the client machine and intercepts requests to the internet and communicates on behalf of the clients as a middleman.

Example usage includes: blocking access to certain content, protecting online identity by using the proxy's IP address.

**Reverse proxy**

A server that sits infront of the web servers intercepting requests from clients such that the client does not communicate directly with the origin server.

Benefits include load balancing, protection from attacks, caching, SSL encryption.