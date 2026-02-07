# Credentials and Secrets

## Parameter store

The Parameter Store is a sub product of the Systems Manager (SSM).

You can create a standard-tier (up to 10,000 parameters at 4 Kb size) or advanced-tier (more than 10,000 parameters at 8 Kb size plus other features) parameter.

You can create a parameter eg. `/app/app_username` that can be String, StringList or Encrypted via KMS.

It can be used to store configuration information.

## AWS Secrets Manager

* It shares functionality with Paramter Store
* It is designed specifically for secrets (passowrds, API keys)
* Usable via Console, CLI, API or SDK i.e. used from within applications
* Supports the automatic rotation of secrets (via Lambda)
* It directly integrates with some AWS products eg. when a secret is rotated, it is changed also inside RDS
* It integrates with IAM
* Secrets are encrypted using KMS

<div align="center">
    <img src="./images/secrets_manager.png" alt="cloud services" width="500" />
</div>