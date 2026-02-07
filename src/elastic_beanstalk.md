# Elasic Beanstalk (EB)

* Uses Cloudformation to do infrastructure actions
* Is a PaaS i.e. vendor handles the infrastructure and user provides the application code
* Is developer-focussed
* Is fully customisable
* It provides managed environments for the developer applications
* An EB "Application" is a contianer containing everything related to the application eg. infrastructure, app code, environments, versions, etc.
* Keep database deployments outside of EBs, decoupling is a best practice


### EB deployment types

* All-in-one: service is interrupted for the duration of the deployment
* Rolling: rolls through the environment upgrading batches 1 at a time. A batch is a replica of the instance eg. 1 of 3 replicas
* Rolling with additional batch: similar to rolling but without dropping any capacity. Another batch is deployed with version 2 of the application alongside the batches with version 1
* Immutable: another full set of instances are created alongside the original i.e. instance x 2
* Traffic-splitting: traffic is split between new and old version similar to blue/green deployment
* Blue/green: allows beta testing version 2 of the application

### Ebextensions (.ebextensions)

* Allow customising the EB environment
* They are based on Cloudformation
* Create a `.ebextensions` folder in an application's source bundle
* Use CFN format (YAML or JSON) inside the `ebextensions` folder to influence the environment

### EB and Docker

* Single container mode (1 container per environment)
    - Dockerfile
    - Dockerrun.aws.json (v1)
    - Docker-compose.yaml
* Multi-container mode
    - EB used the ECS cluster
    - Dockerrun.aws.json (v2)
