# Move to SLSA

The goal of 'Move to SLSA' is to deploy smart contracts on the blockchain via GitHub Actions, and record their provenance (e.g., similar to the [provenance of npm packages](https://github.blog/2023-04-19-introducing-npm-package-provenance/)). Currently, only GitHub Actions and Google Cloud Build are Level 3 certified, ensuring the highest level of build integrity and security for the secure deployment of smart contracts.

![slsa npm](https://doc.zktx.io/images/npm-package-provenance.png)

Our project integrates the SLSA framework to improve the deployment of blockchain smart contracts. This ensures the integrity of smart contracts, and assures users that the package they have downloaded has not been tampered with.

![SLSA on Blockchain](/images/slsa-on-blockchain.svg)

# SLSA on Sui

## Overview

**SLSA on Sui** is a GitHub workflow action developed for the Move language. After building the smart contract source, it creates signed provenance using the generated artifact with [generator_generic_slsa3.yml](https://github.com/slsa-framework/slsa-github-generator/blob/main/internal/builders/generic/README.md). Then, it sends the information required to create a transaction to **SLSA on Blockchain**. The created transaction is returned to SLSA on Sui, where it is verified and then deployed to the blockchain.

## Example

### Publish

#### Workflow Inputs

| Name           | Description                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------- |
| move-directory | The root directory of the Move project refers to the directory containing the `Move.toml` file.   |

#### Example

```bash
name: Publish

on:
  release:
    types:
      - published

permissions:
  actions: read
  contents: write
  id-token: write

jobs:
  build:
    uses: zktx-io/slsa-on-sui/.github/workflows/generator_generic_slsa3.yml@main
    with:
      move-directory: 'smartcontract root folder'
```

### Upgrade

Create an `Upgrade.toml` file in the same location as `Move.toml` and add the necessary information for the upgrade.

| Name        | Description                                            |
| ----------- | ------------------------------------------------------ |
| package_id  | Published Packaged Object ID                           |
| upgrade_cap | Upgrade Object ID _`This input is used only for Sui.`_ |

```toml
[upgrade]
package_id = "Published Package Object ID"
upgrade_cap = "Upgrade Cap Id of Package"
```

## Verification

in development

## Github

Get started with **SLSA on Sui** and learn by [github](https://github.com/zktx-io/slsa-on-sui)
