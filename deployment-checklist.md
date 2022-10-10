# Deployment Checklist

## Deployment

### Deployment via a Gnosis Safe

The deployments of the core contracts of the Morpho protocol and its periphery are done by the 5/9 multisig of the Morpho DAO ([details](https://docs.morpho.xyz/usdmorpho/governance/zodiac-and-progressive-decentralization). They are done through the [AdmoDeployer contract](https://etherscan.io/address/0x08072d67a6f158fe2c6f21886b0742736e925536).```

- [ ] Before submitting the tx, check that the bytecode used corresponds to the correct version (commit, tag) with the correct paramaters (constructor arguments, initialization arguments).
- [ ] For an upgradeable contract, an initialilzation tx MUST be done atomically to initialize the contract and avoid it to be hijacked.
- [ ] Once the tx is submitted on the Gnosis Safe, everyone in the "Deployment Team" MUST check it.
- [ ] Check that the bytecode corresponds to the correct version (commit, tag). 
- [ ] Check that the correct paramaters are passed into the constructor. Or, in the case of an upgradeable contract that an initialization tx is done atomically with the correct parameters.
- [ ] The bytecode MUST be checked on the Gnosis Safe before signing and executing the transaction.
- [ ] Create a simulation via Gnosis and Tenderly, checking that the deployment went through and that state variables are correctly set.
- [ ] Create a fork from this simulation and test the basic features of the contract.
- [ ] Everyone in the "Deployment Team" MUST play with the simulation to make sure that everything is fine.
- [ ] Deploy.

### Deployment via a burner

For less critical pieces of code, a burner might be used for developments. The deployment can the be done via Remix, a js script or a foundry script. The latter being the prefered solution.

- [ ] If a script is used, the script MUST have been reviewed AND approved by the "Solidity Team".
- [ ] For an upgradeable contract, an initialilzation tx MUST be done in the script to initialize the contract and avoid it to be hijacked.
- [ ] Check that if the contract `Ownable`, the ownership is transfered right after deployment in the script.
- [ ] Check that the deployer account has enough funds.
- [ ] The deployer account MUST NOT be a personal account.
- [ ] Deploy.

## After deployment

- [ ] After deployment, the contract MUST be verified on Etherscan asap. The best way is by using [foundry](https://book.getfoundry.sh/forge/deploying?highlight=verify#verifying-a-pre-existing-contract).
- [ ] The deployed contract MUST be checked by the "Deployment Team" (ownership, basic getters, code, etc.).
- [ ] Once verified, the contract MUST be added to Tenderly in the correct projects with the correct labels.
- [ ] Monitor the contracts.
- [ ] Update Immunefi bounty if necessary.

## Notes

- If a contract is `Ownable`, the ownership must be transferred right after deployment. This can be an issue when using a deployer contract as it might be set as owner of the deployed contract. Thus, a logic to transfer the ownership must be implemented in the deployer contract itself as done in this [contract](https://etherscan.io/address/0xd90bbca6a99a53f8b26782edb0b190a7d599c585).
