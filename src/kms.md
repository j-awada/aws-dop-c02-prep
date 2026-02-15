# Key Management Service

Is a regional and public service which manages keys both symmetric and asymmetric. It can also perform cryptographic operations. The keys managed never leave KMS.

A KMS key is logical (i.e. similar to a container) and it contains an ID, date, policy, description and state.

A KMS key can encrypt or decrypt data up to 4KB in size.

By default, keys are created only within a region although multi-region keys can also be created. Those are keys that span multiple regions.

## Data Encryption Keys (DEKs)

Another type of key that KMS can generate. They are generated using a KMS key using the `GenerateDataKey` operation. This generated key can encrypt/decrypt data that is more than 4KB in size.

When created, KMS provides a plaintext version of the key as well as a Ciphertext version.

The way this is used is that the plaintext version is used to encrypt data. The plaintext version is then discarded and the encrypted data is stored along with the Ciphertext version.

## Key policy

Every key has a key policy because you have to explicitly declare that the kwy trusts the identity using it.
