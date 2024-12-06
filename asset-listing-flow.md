## Add priceFeed to priceFeedStore


### Stage 0. Listing decision
Found a token that's attractive from a business perspective.

Checklist:
- [ ] Technically compatible oracle is already deployed (Chainlink/Pyth/Oracle). Or we're capable of creating priceFeed adapter
- [ ] Chaos labs soft-approved that the (token, feed) pair is adequate from the risk perspective.
    - [ ] Chaos approve that feed which we're going to use is ok. (they post pre-analysis like for [PT TWAP oracle](https://discord.com/channels/841203475606011905/1281700097914044416). That means they are familiar with this way of pricing token and have some methodology)

### Stage 1. Developing PriceFeed + Adapter contracts

Checklist:
- [ ] Audit for adapter from 1 firms
- [ ] Audit for price feed from 2 firms
- [ ] Audit uploaded to main branch of security repo
- [ ] Safe checker tool detects the audit

### Stage 2. PriceFeed is ready for usage
Checklist: 
- [ ] Token and priceFeed are added to priceFeedStore (currently priceFeeds.ts of sdk-gov)
- [ ] Ticker for token & priceFeed is added to token.ts of sdk-gov
- [ ] Sdk-gov's main branch has all that changes ^

## Add new collateral token

### Stage 2. Price feed + adapter deploy test env
#### Ilya writes technical specification
<!--     - What can differ from the final Chaos' recommendation:
        - List of dex pools added to trade adapters
        - Compatibility (technical) tokens -->
        
        
What tokens are needed to be included:
- [ ] Tokens that will be used as collateral:
    - [ ] Those with nonzero quota and LT (essential)
    - [ ] Compatibility tokens (0 quota, 0.01% min/maxRate, 0 LT, ZERO_PRICE_FEED if it doesn't have any in priceFeedStore)
        - [ ] Reward tokens (for farming collaterals)
        - [ ] Intermediate swap tokens (e.g. for DAI -> WETH -> USDC route WETH is intermediate)
        - [ ] LP tokens of Curve & Balancer pools that are used for routing

When list of tokens is there:
        
- [ ] List of tokens with priceOracle changes needed
    - [ ] Main feed
        - [ ] Trusted (?)
    - [ ] Reserve feed
- [ ] PoolquotaKeeper parameters
    - [ ] List of tokens that are not quoted in the needed pool yet
        - [ ] minRate, maxRate, quotaLimit (addQuotaToken)
    - [ ] List of tokens that are already quoted, with parameter changes needed
        - [ ]  minRate, maxRate, quotaLimit (setQuotaToken)
- [ ] Credit Manager parameters
    - [ ] List of tokens that are not added as collateral in the needed CM yet
        - [ ] LT (addCollateralToken)
    - [ ] List of tokens that are already quoted, with parameter changes needed
        - [ ] LT (setCollateralToken)
#### Arkhip develops deploy script to test assets

Checklist:
- [ ] Contracts are deployed on a testnet
- [ ] It's possible to open new accounts using script

### Stage 3. Router tests 
- Vanya tests router and confirms that router works fine for all tests


### Stage 4. Integration system tests
- Checks front-end 
- Checks liquidator

Checklist:
- [ ] Dima approves that all FE operations are functional (open, close, swap, withdraw)
- [ ] GO & TS Liquidator works for every account except those flagged

### Stage 5. FE usability checks
Checklist:
- [ ] Dima has all the list of tokens that users will directly interact with (those appearing in collateral lists, strategies, swaps)
- [ ] All logos of new tokens are uploaded to static.app
- [ ] Dima has APR sources for all new tokens that have inherent yield
- [ ] Dima has lists of all points being accrued by new tokens 

### Stage 5. Chaos labs provide final params for GIP
- Chaos send a json generated from risk curators interface (simpler checklist for now)
- Arkhip modifies deploy script with final numbers

### Stage 6. Generating final version of txns

- [ ] Txns are generated on 2 different computers, bytecodes are compared
- [ ] PriceDiffs are checked to exclude bad priceFeed update
- [ ] StateDiffs are checked

### Stage 7. Voting proposal 
Checklist:
- [ ] Attached the same transactions that were generated

### Stage 8. Adding txs to multisig + signing
Done automatically excepting price range updates()

### Stage 9. Execution

### Stage 10. Prod FE displays strategies correctly
Dima needs the following info:
- [ ] APR source & points description
- [ ] Timestamp when the strategy starts being displayed




