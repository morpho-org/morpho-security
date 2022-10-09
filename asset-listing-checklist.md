# Asset Listing Checklist

## Discussion Phase

- [ ] Only assets that are worth it must be listed.
- [ ] Follow [Trail Of Bits integration token checklist](https://github.com/crytic/building-secure-contracts/blob/master/development-guidelines/token_integration.md).
- [ ] Check the ERC20 compliance with [Slither](https://github.com/crytic/slither/wiki/ERC-Conformance).
- [ ] Asset MUST not bring additional risks to Morpho.
- [ ] Check the pool token code and check the differences with the other pool tokens implementation.
- [ ] Check if the asset is a rebasing token (such as stETH).
- [ ] Check the governance discussions and proposal on the underlying pool governance forum that listed this asset and check risk companies report.
- [ ] Check global risk parameters (such as LTV or LT on Aave and collateral factor on Compound.
- [ ] Check if the asset is supply only.
- [ ] Check if the asset has a borrow cap.
- [ ] Check if there exist any other particular treatment on the underlying pool.
- [ ] Do a governance post.

## Before Listing

- [ ] If the asset passed the "Discussion Phase", the asset MUST be tested on a fork. The protocol should behave as usual with this new asset.
- [ ] Test asset with fuzzing.

## Listing

- [ ] Prepare the tx to be signed and executed on a Gnosis Safe.
- [ ] Everyone in the "Deployment Team" MUST verify the tx and its parameters.
- [ ] A simulation MUST be done and everyone MUST check that the parameters are correct after listing.
- [ ] If everything is fine, the tx can be executed.

## After Listing

- [ ] Verify the same thing in production.
- [ ] Test in prod (by supplying, borrowing, etc in production).
