# Asset Listing Checklist

## Discussion Phase

- [ ] Only assets that are worth it must be listed.
- [ ] Follow [Trail Of Bits integration token checklist](https://github.com/crytic/building-secure-contracts/blob/master/development-guidelines/token_integration.md).
- [ ] Check for ERC20 compliance with [Slither](https://github.com/crytic/slither/wiki/ERC-Conformance).
- [ ] Check the code of the pool token and check for any differences from other pool token implementations.
- [ ] Check if the asset is a rebasing token (such as stETH) and ensure that it is handled appropriately.
- [ ] Look through the governance discussions and proposals of the asset in the underlying pool's governance forum and read through any of the asset's risk reports.
- [ ] Check global risk parameters (such as LTV or LT on Aave and collateral factor on Compound).
- [ ] Check if the asset is supply only.
- [ ] Check if the asset has a borrow cap.
- [ ] Check if there exists any other special treatment of the asset in the underlying contracts.
- [ ] Do a governance post.

## Before Listing

- [ ] If the asset passes the "Discussion Phase", the asset MUST be tested on a fork and fuzzed. The protocol MUST behave as usual with this new asset.

## Listing

- [ ] Prepare the transaction to be signed and executed on the 5/9 pre-DAO multisig.
- [ ] Everyone in the "Deployment Team" MUST verify the tx and its parameters.
- [ ] A simulation MUST be done and everyone MUST check that the parameters are correct after listing.

## After Listing

- [ ] Verify that the asset has been listed on Morpho with the correct parameters:
  - [ ] Pool token address token is correct
  - [ ] Reserve factor and P2P index cursor are correct.
  - [ ] P2P and pool indexes have been updated.
  - [ ] Pool token address has been added to the markets created.
  - [ ] The LTV, liquidation threshold, decimals, and liquidation bonus should mirror the underlying pool
- [ ] Test all four accessible functions in production.
