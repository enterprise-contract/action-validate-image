
# Action Validate Image

"Validate" is a robust GitHub Action developed by Enterprise Contract, designed to assess container images for security and compliance. This action has two authentication methods: Long-Lived Public-Key Authentication and Keyless Authentication made possible by Enterprise Contract.
## Keyless Authentication: The Strongest Approach

Keyless Authentication represents the highest level of Sigstore sophistication. Verification is based on the signer's identity, It's made possible by two essential parts:

- **Certificate Identity:** This identifier is crucial for verifying the image-associated certificate.
- **Certificate OIDC Issuer:** The OIDC issuer serves a vital role in validating the identity.

Keyless Authentication significantly enhances security, ensuring image authenticity. The verification process hinges on the signer's identity, providing an innovative solution for robust container image authentication.

## Long-Lived Public-Key Authentication

The Long-Lived Public-Key Authentication method involves a comprehensive three-stage validation process:

1. **Signature Verification:** The action cryptographically verifies image signatures using the provided public key.

2. **Attestation Verification:** It ensures images possess valid and signed attestations, enriching details about image origin and attributes.

3. **Policy Conformance Verification:** This step aligns image attestations with pre-established policies, guaranteeing organizational standards compliance.

For more details:

- [Enterprise Contract Documentation](https://enterprisecontract.dev/docs/ec-cli/main/ec_validate_image.html#_synopsis)
- [Konflux-CI Documentation](https://konflux-ci.dev/architecture/architecture/enterprise-contract.html)


## Environment Variables

To use this action, please configure the following environment variables in your workflow based on the desired authentication method.

### Using Long-Lived Public-Key Authentication

The Long-Lived public-key method offers a straightforward way to integrate with Sigstore. Below are the variables needed:

| Name          | Description                                                                                      | Example                                     |
|---------------|--------------------------------------------------------------------------------------------------|---------------------------------------------|
| Public Key    | The public key for verifying signatures.                                                | `your_public_key_goes_here`                 |
| Policy        | The location of the policy.yaml config file to be used when running Enterprise Contract. A list of standard configs can be found at [here](https://github.com/conforma/config).  | `github.com/conforma/config//slsa3` |
| Image         | Image that is built.                                                                            | `quay.io/redhat-appstudio/ec-golden-image:latest` |

### Using Identity-Based Short-Lived (Keyless) Authentication

This authentication approach is the most robust, as Sigstore authentication is based on the signer's identity rather than a key. The following variables are needed:

| Name          | Description                                                                                      | Example                                     |
|---------------|--------------------------------------------------------------------------------------------------|---------------------------------------------|
| Identity      | A regexp string used to match the certificate identity linked to the image.                                          | `https:\/\/github\.com\/(slsa-framework\/slsa-github-generator\|lcarva\/festoji)\/`                 |
| Issuer        | OIDC issuer for validation purposes.                                                             | `https://token.actions.githubusercontent.com` |
| Image         | Image to be verified.                                                                               | `quay.io/lucarval/festoji:latest` |

Ensure to pick the right method for your needs and set up the variables accordingly.

### Optional (extra-params)
The Enterprise Contract provides a variety of additional options that can seamlessly integrate into this action-validate-image. These extra-params are outlined in the official documentation, which can be found [here](https://enterprisecontract.dev/docs/ec-cli/main/ec_validate_image.html#_options). You have the freedom to incorporate as many as you need. Just simply add `extra-params` into your worflow.

```shell
extra-params: --ignore-rekor --debug
```
## Usage/Examples

If you're eager to experience the benefits of the "Validate" action in your build process, follow these simple steps to get started. By copying and pasting either 'keyless' or  'long-lived' example below to your project's `.github/workflows/` directory, you'll be on your way to enhancing your container image security and compliance.

### Long-Lived Public-Key Authentication Example

**Copy and Paste:** Insert the example snippet below into your `.github/workflows/` directory.
1. Replace the Image with your image URL or file path within the `image` parameter.
2. Set up your key using GitHub vars, following recommended practices. Tutorial can be found [here](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository)
3. Choose a policy that aligns with your requirements and objectives. Policy's can be found [here](https://github.com/conforma/config)
Public-Key Authentication)
```javascript
name: example of action validate using long-lived
on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Run EC Validator
      uses: conforma/action-validate-image@v1.0.18
      with:
        image: quay.io/konflux-ci/ec-golden-image:latest
        key: ${{ vars.PUBLIC_KEY }}
        policy: github.com/conforma/config//slsa3
        extra-params: --ignore-rekor
```

### Identity-Based Short-Lived (Keyless) Example
**Copy and Paste:** Insert the example snippet below into your `.github/workflows/` directory.
1. Replace the Image with your image URL or file path within the `image` parameter.
2. Provide your certificate's identity.
3. Add your certificate's OIDC issuer details.
```javascript
name: example of action validate using keyless
on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Run EC Validator
      uses: conforma/action-validate-image@v1.0.18
      with:
        image: quay.io/lucarval/festoji:latest
        identity: https:\/\/github\.com\/(slsa-framework\/slsa-github-generator|lcarva\/festoji)\/
        issuer: https://token.actions.githubusercontent.com
```

