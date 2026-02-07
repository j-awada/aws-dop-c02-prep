# AWS OpsWorks

* Is a configuration management service that helps you configure and operate applications in AWS when you use Puppet or Chef
* Is an AWS service that provides implementations of Chef and Puppet. Chef and Puppet are automation platforms that allow you to use code to cofigure servers
* Use OpsWorks if you are already using Chef or Puppet

**Chef**

**Puppet**

### OpsWorks modes

OpsWorks functions in 3 modes:

1. Puppet Enterprise: where you can create an AWS managed Puppet master server
2. Chef Automate: you can create AWS managed Chef servers
3. OpsWorks: is an AWS implementation of Chef and is managed by AWS

### OpsWorks components

* Stacks: is the core component, a container of resources which share a similar function
* Layers: represent a specific function in the stack eg. load balancer layer, database layer, etc. Recipes and cookbooks are applied to layers
* Lifecycle Events: are events that run on layers eg. Setup, Configure, Deploy, Undeploy and Shutdown
* Instances: are manages by OpsWorks. 3 types of instances: 24/7, time-based and load-based. OpsWorks supports instance healing
* Apps: can be stored in buckets and are deployed in the app layer

<div align="center">
    <img src="./images/opsworks.png" alt="cloud services" width="500" />
</div>