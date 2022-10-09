# Upgrade Checklist

## General Upgrade

### Discussion Phase

- [ ] In general, upgrades should include only really highly demanded features, safety features or critical fixes. Other considerations such as gas improvements must be discussed in depth before taking any decision.
- [ ] List all consequences on other contracts (integrators, `Lens`, Tokenized vaults, etc.). Some contract might need to be updated to mirror/handle the new logic that will be implemented.

### Development and Test Phase

- [ ] The change MUST be implemented only if it has passed the "Discussion Phase".
- [ ] The change MUST not modify the ABI (else it could break integrations). If changes to the ABI cannot be avoided, then integrators MUST be warned in advance and ideally discussed via governance.
- [ ] The change MUST not break the storage layout in any way. For this, you can use this [tool](https://github.com/Rubilmax/foundry-storage-check) developed by [Rubilmax](https://github.com/Rubilmax) and integrate it in the CI.
- [ ] Changes MUST be thouroughly tested with unit tests.
- [ ] Changes MUST be audited.
- [ ] The upgraded contract MUST be tested on a fork on a the current block. The behavior of the upgraded contract MUST behave accordingly to the changes and this MUST be tested.

### Upgrade Phase

Please refer to the [Deployment Checklist](./deployment-checklist.md) for each contract deployment.

- [ ] First, deploy the implementation contract.
- [ ] Use the `ProxyAdmin` contract to upgrade the current proxy contract. There are 3 parameters to check (address of the proxy to upgrade, address of the new implementation, data that can be used to call a function at upgrade time). 
    - [ ] Check that the first parameter corresponds to the correct proxy address to upgrade.
    - [ ] Check that the second parameter correponds to the correct implementation that has been deployed.
    - [ ] Check that the third parameter is correct. At Morpho, this paramater is in general left empty.
- [ ] A simulation via Gnosis Safe and Tenderly must be done, checking that the correct state variables are updated in the proxy contract.
- [ ] Everyone in the "Deployment Team" MUST check this tx and play with the simulation to make sure everything is fine.

### Monitoring Phase

- [ ] Update Immunefi bounty if necessary.
- [ ] Monitor contract with Tenderly or custom bots.

## Specific Upgrades

For some contracts the Upgrade Phase might be more complex. Here is a list of the adding steps to take care of for some specific contracts/protocols.

## Morpho Upgrade

If the `MorphoStorage` is modified (by adding storage slots):
- Slots MUST be added below every other existing state variables.
- Every contracts inheriting from `MorphoStorage` must be updated (such as `InterestRatesManager`).

### Morpho-Compound Upgrade

If only the `InterestRatesManager` or the `PositionsManager` is updated:
- [ ] The implementation MUST inherits from the deployed `MorphoStorage`.
- [ ] Deploy the implemenation.
- [ ] Set the new contract on Morpho, thanks to setters.

### Morpho-Aave Upgrade

If only the `InterestRatesManager`, the `EntryPositionsManager` or the `ExitPositionsManager` is updated:
- [ ] The implementation MUST inherits from the deployed `MorphoStorage`.
- [ ] Deploy the implemenation.
- [ ] Set the new contract on Morpho, thanks to setters.