# Move to SLSA

The goal of 'Move to SLSA' is to deploy smart contracts on the blockchain via GitHub Actions, and record their provenance (e.g., similar to the [provenance of npm packages](https://github.blog/2023-04-19-introducing-npm-package-provenance/)). Currently, only GitHub Actions and Google Cloud Build are Level 3 certified, ensuring the highest level of build integrity and security for the secure deployment of smart contracts.

![slsa npm](https://docs.zktx.io/images/npm-package-provenance.png)

Our project integrates the SLSA framework to improve the deployment of blockchain smart contracts. This ensures the integrity of smart contracts, and assures users that the package they have downloaded has not been tampered with.

![SLSA on Blockchain](https://docs.zktx.io/images/slsa-on-blockchain.svg)

# SLSA on Sui

## Overview

**SLSA on Sui** is a GitHub workflow action developed for the Move language. After building the smart contract source, it creates signed provenance using the generated artifact with [generator_generic_slsa3.yml](https://github.com/slsa-framework/slsa-github-generator/blob/main/internal/builders/generic/README.md). Then, it sends the information required to create a transaction to **SLSA on Blockchain**. The created transaction is returned to SLSA on Sui, where it is verified and then deployed to the blockchain.

## Example

Through this example, you can learn how to deploy and upgrade smart contracts using SLSA.
The code used here can be found at [serializer](https://github.com/MystenLabs/sui/tree/main/sdk/typescript/test/e2e/data/serializer) and [serializer_upgrade](https://github.com/MystenLabs/sui/tree/main/sdk/typescript/test/e2e/data/serializer_upgrade).

> Note: SLSA on Blockchain requires user input at certain stages.
> SLSA on Blockchain secures the developer’s private key by not using GitHub secrets, instead obtaining signatures from an external wallet. Follow these steps to complete the process:
>
> 1. Trigger the Workflow: Start the deployment process by triggering the workflow.
> 2. Connect Wallet:
>
> - The workflow will provide a link in the `connect-wallet` step.
> - Click on this link to open the front-end interface for wallet connection.
>   ![connect wallet](https://docs.zktx.io/images/connect-wallet.png)
>
> 3. Sign the Transaction:
>
> - In the opened window, connect your external wallet.
> - Follow the prompts to sign the transaction.
>
> 4. Complete the Process:
>
> - After signing, the workflow will automatically continue to deploy the smart contract to the blockchain.

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
