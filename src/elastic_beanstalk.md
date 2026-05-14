# Elasic Beanstalk (EB)

Is a PaaS i.e. the vendor handles the infrastructure and the user provides the application code. It is developer-focussed because it provides managed environments for developer applications.

An EB "Application" is a contianer containing everything related to the application eg. infrastructure, app code, environments, versions, etc. However it is considered a best practice to keep the database deployment outside of EB.

## EB deployment types

* All-in-one: service is interrupted for the duration of the deployment
* Rolling: rolls through the environment upgrading batches 1 at a time. A batch is a replica of the instance eg. 1 of 3 replicas
* Rolling with additional batch: similar to rolling but without dropping any capacity. Another batch is deployed with version 2 of the application alongside the batches with version 1
* Immutable: another full set of instances are created alongside the original i.e. `N` x 2
* Traffic-splitting: traffic is split between new and old version similar to blue/green deployment
* Blue/green: allows beta testing version 2 of the application

## Ebextensions (.ebextensions)

Is a folder in the application's source bundle that allows the customisation of the EB environment. The files inside the `.ebextensions` folder follow a Cloudformation format (YAML or JSON).

## EB and Docker

* Single container mode: a Docker image is deployed which is described in a `Dockerfile` or a `Dockerrun.aws.json` file. This mode runs 1 container per instance.

* Multi-container mode: uses Elastic Container Service (ECS) to deploy multiple Docker containers to an ECS cluster in an Elastic Beanstalk environment.
