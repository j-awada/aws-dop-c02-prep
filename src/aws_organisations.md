## AWS Organisations

This allows larger businesses to manage multiple AWS accounts

Start by taking a standard AWS account which is not in an organisation. Within this account, create an organisation. This account then becomes the management or master account of the organisation. The master account is used to invite other standard accounts into the organisation. The standard accounts that join an organisation become member accounts.

The structure of an organisation is hirarchichal i.e. there is an organisation root containing organisation units.

The organisation has consolidated billing i.e. billing is removed for member accounts. Billing is passed to the management account of the organisation.

New accounts can join an organisation via a link or they can be created directly. When creating or adding a new account to an organisation, a role is created or manually added respectively. This role allows the management account to switch into those new accounts.

### SCP (Service Control Policy)

* A feature of AWS organisations which can be used to restrict AWS accounts
* SCP is a JSON policy that can be attached to an organisation, organisation unit or AWS account
* The management account is never affected by SCPs attached to the organisation. The management account can't be restricted
* SCP does not grant permissions, it only creates boundaries
* The default is Allow, you need to add Deny or remove the default Allow access

Example organisation structure:

```Bash
Root
|
|---> management account
|---> Dev (OU)
       |
       |---> dev account
|
|---> Prod (OU)
       |
       |---> prod account
```

The SCP needs to be enabled.
* `FullAWSAccess` policy is active
* Create a new SCP eg. to deny S3 actions
* Apply the new SCP to the prod OU
* Detach the `FullAWSAccess` SCP from prod OU
* When you switch roles to the prod account, you will not have access to S3
