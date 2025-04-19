# Burve contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the **Issues** page in your private contest repo (label issues as **Medium** or **High**)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

### Q: On what chains are the smart contracts going to be deployed?
Berachain,
Monad,
Base,
Avalanche,
HyperLiquid L1
BSC,
Arbitrum

___

### Q: If you are integrating tokens, are you allowing only whitelisted tokens to work with the codebase or any complying with the standard? Are they assumed to have certain properties, e.g. be non-reentrant? Are there any types of [weird tokens](https://github.com/d-xo/weird-erc20) you want to integrate?
We handle some weird tokens.
Weird tokens not handled:
- Fee on Transfer
- Revert on zero value approvals.

We allow for:
- rebasing tokens
- missing return values
- flash mintable
- approval race protected
- decimals >=6 and <=18

Tokens without a decimal field are carefully selected by whether or not they imply a decimal of 18.
___

### Q: Are there any limitations on values set by admins (or other roles) in the codebase, including restrictions on array lengths?
Owners and vetoers are trusted individuals (that will be decentralized over time).
Owners are expected to set values appropriately. This means:
- fee parameters all represent less than 100%.
- Vertex vaults have acceptable risk tolerances and are expected to not lose token amounts. Currently they can only be ERC4626s.
- The adjustors are properly configured.
- The bgtExchanges are properly configured and are almost always funded with BGT.
- New bgtExchange configurations use the previous one as a backup.
- The station proxy properly collects bgt earnings and allows users to collect it from there.
- The deMinimus in searchParams is smaller than 1e6.
- esX128 for any token is less than 2^24 << 128.


___

### Q: Are there any limitations on values set by admins (or other roles) in protocols you integrate with, including restrictions on array lengths?
Integrated tokens do not change their decimal value.
Integrated vaults properly behave as ERC4626s.
___

### Q: Is the codebase expected to comply with any specific EIPs?
We conform to ERC-2535.
___

### Q: Are there any off-chain mechanisms involved in the protocol (e.g., keeper bots, arbitrage bots, etc.)? We assume these mechanisms will not misbehave, delay, or go offline unless otherwise specified.
Off-chain systems will monitor any moving pegs and update the adjustor accordingly if that information is not available on-chain.
___

### Q: What properties/invariants do you want to hold even if breaking them has a low/unknown impact?
No closure can send out more tokens than its recorded balance.
The "value" of the closure (according to the formulas) can never be more than deMinimus * (number of tokens in the closure) from the target value of the pool times number of tokens in the closure.
___

### Q: Please list any relevant protocol resources.
docs.burve.fi
burve.fi
___

### Q: Additional audit information.
Primarily concerned if there is any potential for loss-of-funds and unfair earning distribution. Whether that be through exploits in logical errors in administration, balance management, or the accounting math. 


# Audit scope

[Burve @ 945f30bfae8afc41af21305ff8c2271ca0ffe6c3](https://github.com/itos-finance/Burve/tree/945f30bfae8afc41af21305ff8c2271ca0ffe6c3)
- [Burve/src/FullMath.sol](Burve/src/FullMath.sol)
- [Burve/src/Timed.sol](Burve/src/Timed.sol)
- [Burve/src/TransferHelper.sol](Burve/src/TransferHelper.sol)
- [Burve/src/integrations/BGTExchange/BGTExchanger.sol](Burve/src/integrations/BGTExchange/BGTExchanger.sol)
- [Burve/src/integrations/BGTExchange/IBGTExchanger.sol](Burve/src/integrations/BGTExchange/IBGTExchanger.sol)
- [Burve/src/integrations/adjustor/DecimalAdjustor.sol](Burve/src/integrations/adjustor/DecimalAdjustor.sol)
- [Burve/src/integrations/adjustor/E4626ViewAdjustor.sol](Burve/src/integrations/adjustor/E4626ViewAdjustor.sol)
- [Burve/src/integrations/adjustor/FixedAdjustor.sol](Burve/src/integrations/adjustor/FixedAdjustor.sol)
- [Burve/src/integrations/adjustor/IAdjustor.sol](Burve/src/integrations/adjustor/IAdjustor.sol)
- [Burve/src/integrations/adjustor/MixedAdjustor.sol](Burve/src/integrations/adjustor/MixedAdjustor.sol)
- [Burve/src/integrations/adjustor/NullAdjustor.sol](Burve/src/integrations/adjustor/NullAdjustor.sol)
- [Burve/src/integrations/pseudo4626/noopVault.sol](Burve/src/integrations/pseudo4626/noopVault.sol)
- [Burve/src/multi/Adjustor.sol](Burve/src/multi/Adjustor.sol)
- [Burve/src/multi/Asset.sol](Burve/src/multi/Asset.sol)
- [Burve/src/multi/Constants.sol](Burve/src/multi/Constants.sol)
- [Burve/src/multi/Diamond.sol](Burve/src/multi/Diamond.sol)
- [Burve/src/multi/Simplex.sol](Burve/src/multi/Simplex.sol)
- [Burve/src/multi/Store.sol](Burve/src/multi/Store.sol)
- [Burve/src/multi/Token.sol](Burve/src/multi/Token.sol)
- [Burve/src/multi/Value.sol](Burve/src/multi/Value.sol)
- [Burve/src/multi/closure/Closure.sol](Burve/src/multi/closure/Closure.sol)
- [Burve/src/multi/closure/Id.sol](Burve/src/multi/closure/Id.sol)
- [Burve/src/multi/facets/LockFacet.sol](Burve/src/multi/facets/LockFacet.sol)
- [Burve/src/multi/facets/SimplexFacet.sol](Burve/src/multi/facets/SimplexFacet.sol)
- [Burve/src/multi/facets/SwapFacet.sol](Burve/src/multi/facets/SwapFacet.sol)
- [Burve/src/multi/facets/ValueFacet.sol](Burve/src/multi/facets/ValueFacet.sol)
- [Burve/src/multi/facets/ValueTokenFacet.sol](Burve/src/multi/facets/ValueTokenFacet.sol)
- [Burve/src/multi/facets/VaultFacet.sol](Burve/src/multi/facets/VaultFacet.sol)
- [Burve/src/multi/vertex/E4626.sol](Burve/src/multi/vertex/E4626.sol)
- [Burve/src/multi/vertex/Id.sol](Burve/src/multi/vertex/Id.sol)
- [Burve/src/multi/vertex/Reserve.sol](Burve/src/multi/vertex/Reserve.sol)
- [Burve/src/multi/vertex/VaultPointer.sol](Burve/src/multi/vertex/VaultPointer.sol)
- [Burve/src/multi/vertex/VaultProxy.sol](Burve/src/multi/vertex/VaultProxy.sol)
- [Burve/src/multi/vertex/Vertex.sol](Burve/src/multi/vertex/Vertex.sol)
- [Burve/src/single/Burve.sol](Burve/src/single/Burve.sol)
- [Burve/src/single/Fees.sol](Burve/src/single/Fees.sol)
- [Burve/src/single/IStationProxy.sol](Burve/src/single/IStationProxy.sol)
- [Burve/src/single/Info.sol](Burve/src/single/Info.sol)
- [Burve/src/single/TickRange.sol](Burve/src/single/TickRange.sol)


