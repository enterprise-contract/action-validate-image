
# EC-action-verifier

EC to be a general purpose tool for people who want to build their software securely. It Validate conformance of container images with the Enterprise Contract
## Documentation

[EC Documentation](https://enterprisecontract.dev/)


## Environment Variables

To run this action, you will need to add the following variables to your workflow.

`public key` Is done with using secret variables using github e.g. ` ${{ secrets.PUBLIC_KEY }}`  [github tutorial](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository)

`Policy` A list of policy that can be used can be found at [Policy's](https://github.com/enterprise-contract/config) e.g. `"https://github.com/enterprise-contract/config/raw/main/slsa3/policy.yaml"`


`Image` Image that is built e.g.`"quay.io/redhat-appstudio/ec-golden-image:latest"`


## Usage/Examples

```javascript
name: TEST REPO
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
      uses: seanconroy2021/EC_GIT@V2
      with:
        image: "quay.io/redhat-appstudio/ec-golden-image:latest"
        key: ${{ secrets.PUBLIC_KEY }}
        policy: "https://github.com/enterprise-contract/config/raw/main/slsa3/policy.yaml"
