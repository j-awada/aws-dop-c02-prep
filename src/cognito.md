# Amazon Cognito

Helps implement secure sign-in and access control for users, AI agents and microservices in minutes.

It supports login with social identity providers and passwordless login using WebAuthn passkeys or SMS and email one-time passwords.

Cognito enables identity federation where you don't need to create custom sign-in code or manage user identities. Users can instead sign in using an external identity provider (IdP) such as Login with Amazon, Facebook, Google or another OpenID Connect (OIDC)-compatible IdP. They can receive an authentication token and exchange the token for temporary security credentials in AWS that map to an IAM role with permissions to use resources in an AWS account.

## How it works / use case

To enable the mobile app to access your AWS resources, you should first register for a developer ID with your chosen IdPs. You can also configure the application with each of these providers. In your AWS account that contains the Amazon S3 bucket and DynamoDB table for the game, you should use Amazon Cognito to create IAM roles that precisely define permissions that the game needs. If you are using an OIDC IdP, you can also create an IAM OIDC identity provider entity to establish trust between your AWS account and the IdP.

In the app’s code, you can call the sign-in interface for the IdP that you configured previously. The IdP handles all the details of letting the user sign in, and the app gets an OAuth access token or OIDC ID token from the provider. Your mobile app can trade this authentication information for a set of temporary security credentials that consist of an AWS access key ID, a secret access key, and a session token. The app can then use these credentials to access web services offered by AWS. The app is limited to the permissions that are defined in the role that it assumes.
