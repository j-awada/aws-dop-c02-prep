# Elastic Load Balancing

## History

There existed v1 load balancers but the most recent ones are v2. Classic load balancers (CLB); introduced in 2009,  are v1 and can load balance HTTP and HTTPS but they aren't layer 7 devices since they lack features to understand HTTP.

Version 2 LBs should now be used and they include:

* Application load balancers (ALB) that support HTTP(S) and WebSocket
* Network load balancers (NLB) that support TCP, TLS and UDP

## About

ELBs accept connections from users and distribute those connections against any registered backend compute. LBs can be private or public and they can distribute traffic across 1 or more AZs.

## Types

**Application LB**

It is a true Layer 7 LB that can listen on HTTP(S) not including SMTP, SSH, etc. protocols. It can convey traffic across various applications using Listener rules.

It can inspect Layer 7 protocol information such as cookies, headers, etc. and make routing decisions based on that information.

ALB are slower than NLB because more levels of the network stack are involved.

**Network LB**

It is a Layer 4 device that can interpret TCP, TLS and UDP with no understanding of HTTP(S) protocols.

NLBs are really fast because they don't need to deal with the upper layers of the networking stack.

**Gateway LB**

* Runs at the OSI network layer, L3
* Can act as a single entry/exit for all traffic

## Session state

Is a piece of server-side information specific to a single user interacting with 1 application.

Session state is hosted externally and not on an instance for eg. in a database. This allows instances to be stateless and in case of failure, session information for a logged-in user is not lost. This is optimal for scaling instances.

### Session stickiness

Stickiness allows us to control which backend instance should be used with a particular user connection. It locks a session to 1 backend instance.

Session stickiness can be enabled on an ELB with a valid duration. The LB (ALB) then generates a cookie called AWSALB delivered to a user's browser. The cookie from the user is provided to the load balancer on every request. The cookie ensures that the requests from a user are forwarded to a specific instance on the backend.

Stickiness might cause a load on backend instances and saturate a server.
