# Asset Listing Checklist

## Discussion Phase

- [ ] Check that the asset is not deprecated on the pool (on Compound: `!isDeprecated`).
- [ ] Check that the asset does not have transfer fees.
- [ ] Follow [Trail Of Bits integration token checklist](https://github.com/crytic/building-secure-contracts/blob/master/development-guidelines/token_integration.md).
- [ ] Check for ERC20 compliance with [Slither](https://github.com/crytic/slither/wiki/ERC-Conformance) and the [Runtime Verification tool](https://ercx.runtimeverification.com/).
- [ ] Check the code of the pool token and check for any differences from other pool token implementations.
- [ ] Check if the asset is a rebasing token (such as stETH) and ensure that it is handled appropriately.
- [ ] Look through the governance discussions and proposals of the asset in the underlying pool's governance forum and read through any of the asset's risk reports.
- [ ] Check the risk parameters of the asset on the underlying pool (LTV or LT on Aave and collateral factor on Compound, liquidation incentive, interest rates model and reserve factor).
- [ ] Check if the asset is supply only (on Compound: `borrowGuardianPaused`; on Aave: `!borrowingEnabled`). If true then pause the borrow and disable the P2P.
- [ ] Check if the asset has a supply or a borrow cap.
- [ ] Check if there exists any other special treatment of the asset in the underlying contracts.
- [ ] Check the token decimals and its price feed. For the heap data structure, amounts cannot exceed `2**96 ~ 7.9e28` (Morpho-AaveV3 only). Thus, low value assets with 18 decimals can be problematic.
- [ ] (Morpho-AaveV3 only) Check if the token has a specific mode (e-mode, isolation mode, siloed borrowing) and make sure that listing this asset cannot break Morpho's state on the pool.
- [ ] Do a governance post.

## Before Listing

- [ ] If the asset passes the "Discussion Phase", the asset MUST be tested (integration test and fuzzing) on a fork. The protocol MUST behave as usual with this new asset.
- [ ] Morpho's liquidation bot MUST be able to liquidate on this asset (if the asset is borrowable).
- [ ] Check that the asset is not paused (on Compound: `isListed && !mintGuardianPaused && !borrowGuardianPaused && !transferGuardianPaused && !seizeGuardianPaused`, on Aave: `isActive && !isFrozen`).

## Listing

- [ ] Prepare the transaction to be signed and executed on the 5/9 pre-DAO multisig.
- [ ] Everyone in the "Deployment Team" MUST verify the tx and its parameters (correct reserve factor and peer-to-peer index cursor). Also, the tx might be sending some dust of underlying tokens to the Morpho contract to avoid any rounding issue. For Morpho-AaveV3, some aTokens of the related assets should be sent to the contract, so that, even if all supplied assets are removed from Morpho, the pool still considers the asset as a collateral for Morpho (unless it's unset by the Morpho DAO later on).
- [ ] A simulation MUST be done and everyone MUST check that the parameters are correct after listing.

## After Listing

- [ ] Verify that the asset has been listed on Morpho with the correct parameters:
  - [ ] Pool token address token is correct.
  - [ ] Reserve factor and P2P index cursor are correct.
  - [ ] P2P and pool indexes have been updated.
  - [ ] Pool token address has been added to the markets created.
  - [ ] The LTV, liquidation threshold, decimals, and liquidation bonus should mirror the underlying pool
- [ ] Test `supply`/`borrow`/`repay`/`withdraw` functions in production.
