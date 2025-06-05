# Umbrella System Parameters Guide

## Contract Addresses

```
Umbrella: 0xD400fc38ED4732893174325693a63C30ee3881a8
RewardsController: 0x4655Ce3D625a63d30bA704087E52B4C31E38188B
DataAggregationHelper: 0xcc8FD820B1b9C5EBACA8615927f2fFc1f74B9dB3
Ethereum Core Pool V3: 0x87870Bca3F3fD6335C3F4ce8392D69350B4fA4E2
AaveOracle: 0x54586bE62E3c3580375aE3723C145253060Ca0C2
StakeUSDC: 0x6bf183243FdD1e306ad2C4450BC7dcf6f0bf8Aa6
StakeUSDT: 0xA484Ab92fe32B143AEE7019fC1502b1dAA522D31
StakeWETH: 0xaAFD07D53A7365D3e9fb6F3a3B09EC19676B73Ce
StakeGHO: 0x4f827A63755855cDf3e8f3bcD20265C833f15033
```

**Note**: Please check for the latest contract addresses in the Address Book: https://search.onaave.com/

## Data Retrieval Steps

### 1. Aggregated Data

`DataAggregationHelper.getTokensAggregatedData(umbrella, aaveOracle)`

- `totalAssets` - current amount of funds deposited into the system and available for slashing
- `targetLiquidity` -  the amount of staked assets according to which the system optimizes rewards; at the target liquidity of total assets staked, the system is distributing maximum rewards.
- `maxEmissionPerSecond` - rate of the token rewards per second at `targetLiquidity` of staked assets. Both when total staked assets are lower or higher than target liquidity, the emission per second of rewards decreases.

For example, if for staked wrapped aUSDC, the `RewardsController` configures a maximum emission per second of 0.1 GHO (8'640 GHO per day), then such amount of tokens per day will be distributed when `totalAssets` is equal to `targetLiquidity`.

- `distributionEnd` - The timestamp marking when reward distribution for a token stops.

- Response details and mapping

    ```json
    Response sample:
    [[0xD15e98f7AD53954fc96A7904738Eb1Ed1f6F0422,100198410,Umbrella Stake Wrapped Aave Base Sepolia USDC V1,stkwaBasSepUSDC.V1,6,1001504984320,50000000000000,true,0xba50Cd2A20f6DA35D788639E581bca8d0B5d4D5f,99982437,USD Coin,USDC,6,160000,1753788774,0x0a215D8ba66387DCA84B284D18c3B4ec3de6E54a,100043658,Tether USD,USDT,6,0,1741892964]
    ```

    ```json
    Mapping:
    {
      "stakeTokenData": {
        "token": "0xD15e98f7AD53954fc96A7904738Eb1Ed1f6F0422",
        "price": 100198410,
        "name": "Umbrella Stake Wrapped Aave Base Sepolia USDC V1",
        "symbol": "stkwaBasSepUSDC.V1",
        "decimals": 6
      },
      "totalAssets": 1001504984320,
      "targetLiquidity": 50000000000000,
      "isStakeConfigured": true,
      "rewardsTokenData": [
        {
          "rewardTokenData": {
            "token": "0xba50Cd2A20f6DA35D788639E581bca8d0B5d4D5f",
            "price": 99982437,
            "name": "USD Coin",
            "symbol": "USDC",
            "decimals": 6
          },
          "maxEmissionPerSecond": 160000,
          "distributionEnd": 1753788774
        },
        {
          "rewardTokenData": {
            "token": "0x0a215D8ba66387DCA84B284D18c3B4ec3de6E54a",
            "price": 100043658,
            "name": "Tether USD",
            "symbol": "USDT",
            "decimals": 6
          },
          "maxEmissionPerSecond": 0,
          "distributionEnd": 1741892964
        }
      ]
    }
    ```

### 2. Reserve Identification

`Umbrella.getStakeTokenData(stakeToken)` → returns `reserve`

- `reserve` - The underlying token associated with a given `StakeToken`

### 3. Reserve Deficits

`Umbrella.getDeficitOffset(reserve)` → `deficitOffset`

- `deficitOffset` - variable that sets a buffer below which no slashing occurs. Over time, this variable will decrease, and when it becomes equal to `reserveDeficit`, stakers will be at risk of slashing.

`Umbrella.getPendingDeficit(reserve)` → `pendingDeficit`

- `pendingDeficit` - slashed funds that will be used to cover the Aave Pool reserve deficit. They're no longer withdrawable by stakers and are sent to the Aave Collector further eliminate the pool deficit.

`Pool.getReserveDeficit(reserve)` → `reserveDeficit`

- `reserveDeficit` - the amount of missing funds (debt) in the Aave pool that must be covered. For example, if there's a 1,000 USDC shortfall in the Aave v3 pool, the reserveDeficit is 1,000 USDC.

### 4. Per User StakeTokens and Rewards

- `RewardsController.calculateCurrentUserRewards(asset,user)` - calculates and returns the total amount of rewards accrued by the specified `user` for the given `asset`.
- `UmbrellaStakeToken.balanceOf(user)` - returns the number of StakeTokens held by the specified `user`.

### Parameters that could be changed via Permissioned Payloads Controller:

- `maxEmissionPerSecond`
- `distributionEnd`
- `rewardPayer` - The address configured as the source of reward tokens to be pulled by Umbrella and distributed to stakers.

  Currently, **rewardPayer** cannot be viewed on Etherscan because no getter has been implemented. It typically doesn't change often, and its initial value was defined in proposal 320: https://vote.onaave.com/proposal/?proposalId=320 to Collector.
