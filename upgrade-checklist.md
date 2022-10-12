# Upgrade Checklist

## General Upgrade

### Discussion Phase

- [ ] In general, upgrades should include only really highly demanded features, safety features or important fixes, and be discussed in depth.
- [ ] List all consequences on the external environment: on and off-chain integrators, front-end, `Lens`, tokenized vaults, liquidators. Some contract might need to be updated to mirror/handle the new logic that will be implemented.

### Development and Test Phase

- [ ] The change MUST be implemented only if it has passed the "Discussion Phase".
- [ ] The change MUST NOT modify the ABI (else it could break integrations). If changes to the ABI cannot be avoided, then integrators MUST be warned in advance and ideally discussed via governance.
- [ ] The change MUST NOT break the storage layout in any way. To continuously check this condition in the development process, this [tool](https://github.com/Rubilmax/foundry-storage-check) developed by [Rubilmax](https://github.com/Rubilmax) integrated into the CI can help.
- [ ] Changes MUST be thouroughly tested with unit tests.
- [ ] Changes MUST be audited.
- [ ] The upgrade MUST be tested on a fork, on the real deployed protocol, with general tests and upgrade-specific tests.

### Upgrade Phase

- [ ] First, deploy the implementation contract (refer to the [Deployment Checklist](./deployment-checklist.md) for this).
- [ ] Use the `ProxyAdmin` contract to upgrade the current proxy contract. There are 3 parameters to check:
    - [ ] The first parameter MUST corresponds to the address of the proxy to upgrade.
    - [ ] The second parameter MUST corresponds to the address of the new implementation.
    - [ ] The third parameter, which is the data of the upgrade, MUST generally be empty.
- [ ] A simulation via Gnosis Safe and Tenderly must be done, checking that the correct state variables are updated in the proxy contract.
- [ ] Everyone in the "Deployment Team" MUST check this tx and play with the simulation to make sure everything is fine.

### Monitoring Phase

- [ ] If some changes are made to governance functions, update the operator accordingly.
- [ ] Monitor contract with Tenderly or custom bots.
- [ ] Update Immunefi bounty if necessary.
- [ ] Update the documentation.

## Specific Upgrades

For some contracts the Upgrade Phase might be more complex. Here is a list of the adding steps to take care of for some specific contracts/protocols.

## Morpho Upgrade

If the `MorphoStorage` is modified (by adding storage slots):
- Slots MUST be added below every other existing state variables.
- Every contracts inheriting from `MorphoStorage` must be updated (such as `InterestRatesManager`).

### Morpho-Compound Upgrade

If only the `InterestRatesManager` or the `PositionsManager` is updated:
- [ ] The implementation MUST inherits from the deployed `MorphoStorage`.
- [ ] Deploy the implementation.
- [ ] Set the new contract on Morpho, thanks to setters.

### Morpho-Aave Upgrade

If only the `InterestRatesManager`, the `EntryPositionsManager` or the `ExitPositionsManager` is updated:
- [ ] The implementation MUST inherits from the deployed `MorphoStorage`.
- [ ] Deploy the implementation.
- [ ] Set the new contract on Morpho, thanks to setters.