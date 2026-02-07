# Advanced Identity and Permissions

## Security Token Service (STS)

STS generates temporary credentials when the `sts::AssumeRole*` operation is used to gain access to temporary credentials. This happens behind the scene.

This is similar to access keys, however these credentials are short-term and they expire after a certain duration.

The credentials generated are used to gain access to AWS resources. The credentials are requested by another identity or service via IAM or external.

<div align="center">
    <img src="./images/sts.png" alt="cloud services" width="500" />
</div>

## Boundary policies

They use JSON documents like identity policies but they don't grant any permissions only limit what permissions an identity can receive.

## AWS policy evaluation logic order

1. Explicit deny
2. SCPs (service control policies)
3. Resource policies
4. Permissions buondaries
5. Session policies
6. Identity policies
