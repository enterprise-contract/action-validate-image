
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
| Policy        | The location of the policy.yaml config file to be used when running Enterprise Contract. A list of standard configs can be found at [here](https://github.com/enterprise-contract/config).  | `"github.com/enterprise-contract/config//slsa3"` |
| Image         | Image that is built.                                                                            | `"quay.io/redhat-appstudio/ec-golden-image:latest"` |



## Usage/Examples

If you're eager to experience the benefits of the "Validate" action in your build process, follow these simple steps to get started. By copying and pasting the example below to your project's `.github/workflows/` directory, you'll be on your way to enhancing your container image security and compliance.

**Copy and Paste:** Insert the example snippet below into your `.github/workflows/` directory.

**Customize Your Parameters:** Tailor the example to your specific needs:
1. Replace the Image with your image URL or file path within the `image` parameter.
2. Set up your key using GitHub vars, following recommended practices. Tutorial can be found [here](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository)
3. Choose a policy that aligns with your requirements and objectives. Policy's can be found [here](https://github.com/enterprise-contract/config)

You are now ready to run the validate action.

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
        policy: "github.com/enterprise-contract/config//default"
