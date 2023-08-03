
# Action Validate Image

"Validate" is a custom GitHub Action made by Enterprise Contract that validates container images for security and compliance. It performs a three-stage validation process:

1. **Signature Verification:** Checks if the image is signed and verifies the signature with a provided public key.

2. **Attestation Verification:** Ensures the image has signed and valid attestations, providing additional information about its source.

3. **Policy Conformance Verification:** Verifies that the image's attestations comply with defined policies, ensuring adherence to organizational standards.

* [Read more](https://enterprisecontract.dev/docs/ec-cli/main/ec_validate_image.html#_synopsis) 
* [Read more](https://redhat-appstudio.github.io/book/book/enterprise-contract.html#:~:text=EC%20CLI,or%20violations%20produced)



## Environment Variables

To run this action, you will need to add the following variables to your workflow.
| Name          | Description                                                                                      | Example                                     |
|---------------|--------------------------------------------------------------------------------------------------|---------------------------------------------|
| Public Key    | The public key for verifying signatures.                                                | `"your_public_key_goes_here"`                 |
| Policy        | A list of policy that can be used can be found at [Policy's](https://github.com/enterprise-contract/config).  | `"github.com/enterprise-contract/config//minimal"` |
| Image         | Image that is built.                                                                            | `"quay.io/redhat-appstudio/ec-golden-image:latest"` |



## Usage/Examples

```javascript
name: example of action validate image
on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Run EC Validator
      uses: enterprise-contract/action-validate-image@v1
      with:
        image: "quay.io/redhat-appstudio/ec-golden-image:latest"
        key: ${{ vars.PUBLIC_KEY }}
        policy: "https://github.com/enterprise-contract/config/raw/main/slsa3/policy.yaml"
