# SDLC Automation

A development pipeline can be generally broken down to the following stages:

1. code
2. build
3. test
4. deploy

AWS provides a range of tools that support all of those stages, for eg.

* CodeCommit similar to GitHub
* CodeBuild which can handle the build and test processes
* CodeDeploy which supports code deployment within AWS

These AWS tools are isolated but they can be configured together using AWS CodePipeline which orchestrates the development pipeline.

A pipeline is linked to 1 branch in a repository eg. a `main` pipeline and a `dev` pipeline.

## CodeDeploy

Can deploy code to:

* EC2 instances
* Elastic Beanstalk
* AWS OpsWorks
* Amazon ECS

It can be used to deploy using CloudFormation.

## Exam concepts

CodeBuild is influenced by `buildspec.yml` file and CodeDeploy is influenced by `appspec.yml` or `appspec.json`.
