# Umbrella Protocol FAQ


## I'm staking GHO on the legacy Aave Safety Module, what should I do?

It depends.

You can stay on the legacy stkGHO without any more risk of slashing and a cooldown of 20 days, but with lower rewards.

Or you can migrate to Umbrella stkGHO, with higher rewards but risk of slashing and 20 20-day cooldown. If you opt for this, you need to do the following once the governance proposal is approved and executed:

1. Cooldown (required step, but you don't need to use any cooldown period) on the legacy stkGHO.
2. Unstake your GHO.
3. Stake your GHO on Umbrella, for example, on [stake.onaave.com](https://stake.onaave.com), an instance of the official [Aave Umbrella UI](https://github.com/aave-dao/aave-umbrella-ui) (owned by the DAO), run by BGD Labs.

## I'm staking AAVE and ABPT on the legacy Safety Module (stkAAVE, stkABPT), anything I should be aware of, or that I need to do?

On this initial activation phase of Umbrella, no action needs to be taken if you are staking on stkAAVE or stkABPT. The only aspects you need to be aware of are that the slashing percentage (how much the system can slash from your staked balance) is reduced from 30% to 20%, and that rewards are slightly reduced too. But the system remains fully functional.

## Which risks do I need to be aware of when staking on Umbrella?

The primary Umbrella risk is the risk of slashing, which occurs if deficit accrues in the associated asset of the connected Aave pool (USDC on v3 Ethereum Core, for instance, if you are staking aUSDC). Historically, the Aave DAO has never slashed stakers of the previous Safety Module, and Umbrella has a mechanism of offset by which the DAO will always cover first-loss up to X amount (e.g., 100'000 in the case of USDT or USDC). But you should understand that it can happen. Additionally, like any smart contract system, there can potentially be bugs that create any type of problem. This has been minimized by all the security procedures disclosed in this forum post and the proposal, including all our internal processes of development at BGD, and four external security audits.

## Can you elaborate on what the Umbrella deficit offset means in simple terms?

For example, on this initial proposal, on staked USDT, the DAO configures an offset of 100'000 USDT. What that means is that more than 100'000 USDT bad debt on the Aave v3 Core pool should be accrued before a single USDC staked asset gets slashed. Basically, the DAO steps up to that amount first to cover any loss, before stakers.

## So by staking on Umbrella, I will always earn the APY defined in the previous forum posts?

Not exactly. Umbrella yield has two components:

1. When staking any non-GHO token (USDC, USDT, WETH initially), you will be earning aToken yield, e.g., ~4% on aUSDT as of the 1st of June. This will accrue automatically in your staked assets, as they will grow in value if no slashing happens and there is yield on Aave.

2. Additionally, for assuming the slashing risk Umbrella gives you rewards continuously. These can be modified by the DAO over time, but it should always be an extra yield percentage on top of the basic Aave yield. To understand the dynamics of how the Umbrella Emission Curve works, we highly recommend reading the explanation, but as a high-level rule of thumb: if Target liquidity would be 100m USDC and the configured rewards at that point would cause a 5% extra yield, that means that whenever nobody is staking, yield would be 10% (double), and if total staked is well above the 100m USDC, yield percentage will be lower (but always more than what you earn just supplying on Aave).

## How do I accrue the Umbrella rewards? And how do I need to claim them?

Once staking, you accrue Umbrella rewards continuously, and you can claim them at any point you want, but doing a blockchain transaction, for example, via [stake.onaave.com](https://stake.onaave.com). The system is fully on-chain.

## I don't have WETH, but only ETH in my wallet. Can I still stake?

Yes, if you use [stake.onaave.com](https://stake.onaave.com), the UI will build the transaction to wrap your ETH into WETH before staking.

## What do I need to do exactly to unstake from Umbrella?

The flow is very similar to the legacy Safety Module: first, you activate the cooldown for the amount you are currently staking. After waiting for the cooldown, currently configured to 20 days, you can unstake your assets. It is not part of the system, but as Umbrella is tokenized, it is also possible that a secondary market for staked Umbrella tokens will appear, which would allow you to get the underlying without cooldown (but probably for a fee).

## I have different assets than the ones enabled on this proposal, or I am using Aave on another network, non-Ethereum, will I be able to use Umbrella?

The plan is to expand Umbrella to at least part of the other Aave pools in the very short future, to some of the assets, as not all of them require staking (if they don't accrue deficit). So yes, even if you don't have USDC, USDT, ETH, or GHO on Ethereum to stake now, with some other assets in the future, you should be able to, always depending on the Aave governance.

## I see that when staking on Umbrella, my aTokens don't increase in balance, but in value, how so?

Technically, the assets you stake in Umbrella are wrapped aToken (previously known as [stata/static a tokens](https://github.com/aave-dao/aave-v3-origin/tree/main/src/contracts/extensions/stata-token)). These are just a special version of aTokens growing in value (exchange rate) and not in balance. Still, in terms of yield, both are exactly equivalent.

If you use [stake.onaave.com](https://stake.onaave.com), the interface will recognize any type of underlying asset and transparently wrap it appropriately for you. This means that no matter if you have e.g. USDT, aUSDT (rebasing, growing in balance), or waUSDT (wrapped already), the interface will allow you to stake.

On unstaking, the interface will also allow you to choose which type of underlying to want to receive. In the previous example USDT, aUSDT or waUSDT.

## I am staking aUSDT in Umbrella, the UI shows me $1000, but that being only ~888 aUSDT staked, how so?

What you are staking is wrapped aTokens, based on exchange rate. It means that 1 wrapped aUSDT has a higher value than USDT or rebasing (balance growing, non-wrapped aUSDT), and so your ~888 staked aUSDT value is $1000.

## What is the role of BGD Labs on the project?

As service providers to the Aave DAO, we designed and developed Umbrella. But as with another smart contract of the Aave DAO, we don't have any type of control over the instances that will be activated with the governance proposal, so our role going forward is to monitor that everything works properly and securely, together with suggesting improvements and supporting other service providers working around Umbrella. Additionally, we developed and ran one instance of the official [Aave Umbrella UI owned by the DAO](https://github.com/aave-dao/aave-umbrella-ui) on [stake.onaave.com](https://stake.onaave.com), but this is totally permissioned software that everybody else can run, and only helps users of the Umbrella smart contracts build transactions and visualize data.