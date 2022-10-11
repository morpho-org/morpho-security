# Deployment Checklist

## Deployment

### Deployment via a Gnosis Safe

The deployments of the core contracts of the Morpho protocol and its periphery are done by the 5/9 multisig of the Morpho DAO ([details](https://docs.morpho.xyz/usdmorpho/governance/zodiac-and-progressive-decentralization)). They are done through the [AdmoDeployer contract](https://etherscan.io/address/0x08072d67a6f158fe2c6f21886b0742736e925536).

- [ ] Before submitting the transaction, check that the bytecode used corresponds to the correct version (commit, tag) with the correct paramaters (constructor and initializer arguments).
- [ ] For an upgradeable contract, an initialization tx MUST be done atomically to initialize the contract to prevent it from being hijacked.
- [ ] Once the transaction is submitted on the Gnosis Safe, everyone in the "Deployment Team" MUST check the transaction details and bytecode.
- [ ] Check that the bytecode corresponds to the correct version (commit, tag).
- [ ] Create a simulation via Gnosis and Tenderly, checking that the deployment goes through and that state variables are set correctly.
- [ ] Create a fork from this simulation and test the basic features of the contract.
- [ ] Everyone in the "Deployment Team" MUST check contract features in the simulation.

### Deployment via a burner

For less critical pieces of code, a burner might be used for developments. The deployment can the be done via Remix, a js script or a foundry script. The latter being the preferred solution.

- [ ] If a script is used, the script MUST have been reviewed AND approved by the "Deployment Team".
- [ ] For an upgradeable contract, an initialization transaction MUST be done in the script to initialize the contract and prevent it from being hijacked.
- [ ] Check that if the contract `Ownable`, the ownership is transferred right after deployment in the script.
- [ ] Check that the deployer account has enough funds.
- [ ] The deployer account MUST NOT be a personal account.

## After deployment

- [ ] After deployment, the contract MUST be verified on Etherscan asap. The best way is by using [foundry](https://book.getfoundry.sh/forge/deploying?highlight=verify#verifying-a-pre-existing-contract).
- [ ] The deployed contract MUST be checked by the "Deployment Team" (ownership, basic getters, code, etc.).
- [ ] Once verified, the contract MUST be added to Tenderly in the correct projects with the correct labels.
- [ ] If ther are some governance functions, update the operator accordingly.
- [ ] Set up monitoring for the contracts:
  - [ ] Add governance event tracking to Tenderly Alerts linked to the correct Slack channel.
  - [ ] Add monitoring of the contract flow according to the data team.
  - [ ] Add revert alerts from Tenderly to the correct slack channel.
  - [ ] Check the dApps integrations and update them accordingly.
  - [ ] Add/Update the signatures allowed to the Operator contract if needed.
- [ ] Update Immunefi bounty if necessary.
- [ ] Update the documentation.

## Notes

- If a contract is `Ownable`, the ownership must be transferred right after deployment. This can be an issue when using a deployer contract as it might be set as owner of the deployed contract. Thus, a logic to transfer the ownership must be implemented in the deployer contract itself like in this [contract](https://etherscan.io/address/0xd90bbca6a99a53f8b26782edb0b190a7d599c585).
