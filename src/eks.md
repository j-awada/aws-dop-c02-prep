# Elastic Kubernetes Service

Is a fully managed service that simplifies running, securing and scaling Kubernetes applications on AWS without needing to install or maintain your own control plane. It eliminates operational overhead by automatically managing and scaling the Kubernetes master nodes.

The control plance is distibuted and managed across multiple AZs for high availability. Compute options include running workloads using EC2, Fargate or with EKS Auto Mode.

## Storage permissions

In EKS, persistent storage is often provisioned using Elastic Block Store (EBS) volumes. The EBS Container Storage Interface (CSI) driver is responsible for integrating Kubernetes with EBS enabling dynamic provisioning and management of volumes as per application requirements.

The EBS CSI driver requires appropriate permissions to dynamically provision and manage EBS volumes. These permissions are managed using an IAM role often attached to the driver itself. AWS recommends assigning an IAM role to the EBS CSI driver with the necessary permissions to provision and manager EBS volumes. This ensures that the driver can perform operations such as creating volumes, attaching them to nodes and managing lifecycle events.
