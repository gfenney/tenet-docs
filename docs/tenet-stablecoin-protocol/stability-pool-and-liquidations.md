---
sidebar_position: 3
---

# Stability Pool and Liquidations

### What is the Stability Pool?

The Stability Pool is the first line of defence in maintaining system solvency. It achieves that by acting as the source of liquidity to repay debt from liquidated CLIPs—ensuring that the total LSDC supply always remains backed.

When any CLIP is liquidated, an amount of LSDC corresponding to the remaining debt of the CLIP is burned from the Stability Pool’s balance to repay its debt. In exchange, the **entire collateral** from the CLIP is transferred to the Stability Pool.&#x20;

The Stability Pool is funded by users transferring LSDC into it (called Stability Providers). Over time Stability Providers lose a pro-rata share of their LSDC deposits, while gaining a pro-rata share of the liquidated collateral. However, because CLIPs are likely to be liquidated at just below `125%` collateral ratios, it is expected that Stability Providers will receive a greater dollar-value of collateral relative to the debt they pay off.

### Why should I deposit LSDC to the Stability Pool?

Stability Providers will make liquidation gains ([see below](broken-reference)), receive minting and redemption fees and are rewarded with extra Tenet incentives.&#x20;

### What are liquidations?

To ensure that the entire stablecoin supply remains fully backed by collateral, CLIPs that fall under the minimum collateral ratio of `125%` will be closed (liquidated).

The debt of the CLIP is canceled and absorbed by the Stability Pool and its collateral distributed among Stability Providers.

The owner of the CLIP still keeps the full amount of LSDC borrowed but loses `~20%` value overall hence it is critical to always keep the ratio above `125%`.

### Who can liquidate CLIPs?&#x20;

Anybody can liquidate a CLIP as soon as it drops below the Minimum Collateral Ratio of `125%`. The initiator receives a gas compensation (`10 LSDC` + `0.5%` of the CLIPs collateral) as reward for this service.

### How am I compensated for liquidating a CLIP?

The liquidation of CLIPs is connected with certain gas costs which the initiator has to cover. The cost per CLIP was reduced by implementing batch liquidations of up to 160 **-** 185 CLIPs but with the aim of ensuring that liquidations remain profitable even in times of soaring gas prices the protocol offers a gas compensation given by the following formula:

`gas compensation = 10 LSDC + 0.5% of` CLIPs `collateral (ETH)`

The `10 LSDC` is funded by a [Liquidation Reserve](broken-reference) while the variable `0.5%` part comes from the liquidated collateral, slightly reducing the liquidation gain for Stability Providers. 

The issue of high gas prices is not present in the Tenet network where typical transaction fees are orders of magnitude lower than on Ethereum mainnet for which this original codebase was designed.&#x20;

### How do I benefit as a Stability Provider from liquidations?

As liquidations happen just below a collateral ratio of `125%`, you will most likely experience a net gain whenever a CLIP is liquidated.&#x20;

Let’s say there is a total of `1,000,000 LSDC` in the Stability Pool and your deposit is `100,000 LSDC`.&#x20;

Now, a CLIP with debt of `200,000 LSDC` and collateral of `155 twstETH` is liquidated at an `twstETH` price of `$1600`, and thus at a collateral ratio of `124% (= 100% * (155 * 1600) / 200,000)`. Given that your pool share is `10%`, your deposit will go down by `10%` of the liquidated debt (`20,000 LSDC`), i.e. from `100,000` to `80,000 LSDC`. In return, you will gain `10%` of the liquidated collateral, i.e. `15.5 twstETH`, which is currently worth `$24,800`. Your net gain from the liquidation is `$4,800`.&#x20;

Note that depositors can immediately withdraw the collateral received from liquidations and sell it to reduce their exposure to `twstETH`, if the USD value of `twstETH` is expected to decrease.

### Can I withdraw my deposit whenever I want?

As a general rule, you can withdraw the deposit made to the Stability Pool at any time. There is no minimum lockup duration. However, withdrawals are temporarily suspended whenever there are liquidatable CLIPs with a collateral ratio below `125%` that have not been liquidated yet.

### What oracle are you using to determine the price of collateral?

At the beginning, the protocol will use its own oracle service. While this is a centralized approach, the required steps to provide security and redundancy to the oracle are applied.&#x20;

We plan to switch to a decentralized oracle solution as soon as possible.&#x20;

### Can I lose money by depositing funds to the Stability Pool?

While liquidations will occur at a collateral ratio well above `100%` most of the time, it is theoretically possible that a CLIP gets liquidated below `100%` in a flash crash or due to an oracle failure. In such a case, you may experience a loss since the collateral gain will be smaller than the reduction of your deposit.&#x20;

If LSDC is trading above `$1`, liquidations may become unprofitable for Stability Providers even at collateral ratios higher than `100%`. However, this loss is hypothetical since LSDC is expected to return to the peg, so the “loss” only materializes if you had withdrawn your deposit and sold the LSDC at a price above `$1`.

Please note that although the system is diligently audited, a hack or a bug that results in losses for the users can never be fully excluded.

### What happens if the Stability Pool is empty when liquidations occur?&#x20;

If the Stability Pool is empty, the system uses a secondary liquidation mechanism called redistribution. In such a case, the system redistributes the debt and collateral from liquidated CLIPs to all other existing CLIPs. The redistribution of debt and collateral is done in proportion to the recipient CLIPs collateral amount.&#x20;
