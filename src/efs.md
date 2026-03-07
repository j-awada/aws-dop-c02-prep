# Elastic File System (EFS)

Provides network-based filesystems that can be mounted within Linux EC2 instances and can be shared by multiple instances at once. This helps the EC2 instances be more stateless.

EFS is an AWS implementation of a common file storage standard called NFS (version 4) network filesystem. It runs inside a VPC.

Filesystems created in EFS are POSIX-based.

An EFS is made available inside a VPC via mount targets. Mount targets have IP addresses within a subnet in an AZ.

* EFS is Linux only
* It offers 2 performance modes: General Purpose and Max I/O
* It has 2 throughput modes: Bursting and Provisioned
* It has 2 storage classes: Standard and Infrequent Access (IA)

## FSx for Windows File Server

Is a shared filesystem product that handles the implementation in a different way than EFS. It supports Windows environments on AWS.

It integrates with Directory Service or Self-Managed Active Directory.

It can be Single or Multi-AZ within a VPC.

## FSx for Lustre

It is a managed implementation of the Lustre filesystem designed for high-performance computing (HPC) workloads. It supports both Linux and POSIX-based filesystems.

FSx is available over VPN or Direct Connect.

Can be provisioned via 2 modes:

* Scratch: optimised for short-term workloads, no resilience or high-availability but fast performance
* Persistent: longer term, HA in 1 AZ and self-healing
