# Asset Risk Assessment: \[Protocol Asset]


### (One sentence summary of the report)

**Useful Links**

- Protocol Website

  - [Reserve Protocol](https://reserve.org/)
  - [Register.app (Explorer/UI for RTokens)](https://register.app/)

- Docs

  - [Reserve Protocol](https://reserve.org/protocol/)
  - [Electronic Dollars (eUSD) Token Details](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)
  - [Electronic Dollars (eUSD) Whitepaper](https://mobilecoin.com/files-uploads/2022/10/MobileCoin_Stablecoin_Whitepaper_Final_Edits.pdf)

- Github

  - [Reserve Protocol](https://github.com/reserve-protocol/protocol)

- Important contracts

  - [eUSD ](https://etherscan.io/address/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)
  - [eusdRSR](https://etherscan.io/address/0x18ba6e33ceb80f077DEb9260c9111e62f21aE7B8)
  - [RSR](https://etherscan.io/address/0x320623b8e4ff03373931769a31fc52a4e78b5d70#code)

- [Access Control Google Sheet](https://docs.google.com/spreadsheets/d/1_tdf1WDr6QMAIVZcEJpMb1Pc43b0W5qRZeqq0ByeH6Y/edit?usp=sharing)

- Governance (Multisig address, gov contract, gov reference)

  - [eUSD Guardian (Governor Alexios) (GnosisSafeProxy)](https://etherscan.io/address/0x576ca79e46171a2B3E26F13a4334940eBcD72164#code)
  - [eUSD (Owner Address, TimelockController)](https://etherscan.io/address/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c#code)

- Market Data (Curve pool, analytics)

  - [eUSD-3CRV Pool](https://etherscan.io/address/0xaeda92e6a3b1028edc139a4ae56ec881f3064d4f)
  - [eUSD-3CRV Gauge](https://etherscan.io/address/0x8605dc0c339a2e7e85eea043bd29d42da2c6d784)

- [MobileCoin Electronic Dollar Auditor](https://auditor.mobilecoin.foundation/)

-


## TLDR

In light of a recent forum post to add eUSD+FraxBP to the gauge controller, posted on[ March 6th, 2023](https://gov.curve.fi/t/proposal-to-add-eusd-fraxbp-gauge/8938), and subsequently deployed on [Curve](https://curve.fi/#/ethereum/pools/factory-v2-277/deposit) and [Convex](https://www.convexfinance.com/stake), this article aims to provide information about the functioning of eUSD (Electronic Dollars). To unpack Electronic Dollars ([eUSD](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)), we’ll start with an overview of MobileCoin, a protocol for [confidential tokens that enables payment on mobile devices](https://mobilecoin.com/files-uploads/2022/10/MobileCoin_Stablecoin_Whitepaper_Final_Edits.pdf). We then introduce [Reserve Protocol](https://reserve.org/en/protocol/), a platform that enables permissionless creation of stablecoins, with Electronic Dollars (eUSD) as one of many stablecoins on the platform.

# MobileCoin Introduction

MobleCoin started in 2017 as an attempt to [develop and integrate fast, private and easy-to-use cryptocurrency](https://mobilecoin.com/files-uploads/2022/09/MobileCoin_White_Paper.pdf) into mobile apps like WhatsApp or Signal. MobileCoin is a [directed acrylic graph (DAG) cryptocurrency blockchain](https://mobilecoin.com/files-uploads/2022/09/Mechanics-of-MobileCoin-v0-0-39-preview-10-11.pdf) that draws its roots from the Stellar Consensus Protocol and Monero, a cryptocurrency focused on privacy. Because of its focus on privacy, earlier technical developments from 2021 focused on cryptographic concepts like [ring signatures and secure enclaves](https://mobilecoin.com/files-uploads/2022/09/Mechanics-of-MobileCoin-v0-0-39-preview-10-11.pdf).

**Electronic Dollars (eUSD)**

A year later in 2022, MobileCoin release the [white paper](https://mobilecoin.com/files-uploads/2022/10/MobileCoin_Stablecoin_Whitepaper_Final_Edits.pdf) for Electronic Dollars (eUSD), the first asset to natively use MobileCoin’s confidential tokens functionality, making it the first private digital dollar, circumventing the need to use “mixers'' as has been done on Ethereum and Bitcoin. 

MobileCoin enables minting, burning, governorship and integration with Gnosis Safe for stablecoins that are backed by collateral assets on Ethereum as is the case with eUSD. There are processes for wrapping and unwrapping eUSD from MobileCoin to Ethereum and vice versa, using a bridge with [ed25519](https://ed25519.cr.yp.to/) public keys and an elliptic-curve signature system in either direction by [pre-approved parties](https://mobilecoin.com/files-uploads/2022/10/MobileCoin_Stablecoin_Whitepaper_Final_Edits.pdf). 

There is a [1:1 relationship](https://auditor.mobilecoin.foundation/) between wrapped eUSD tokens on the MobileCoin blockchain and eUSD ERC20 tokens stored in a [Gnosis Safe](https://app.safe.global/balances?safe=eth:0x30DA4EB397215cF407C46854CA7188f4e60F3402) multisig on Ethereum. Minting and burning of wrapped eUSD is [verifiable](https://auditor.mobilecoin.foundation/) on the MobileCoin blockchain, while correspondent deposits and withdrawals from the custodian multisig are verifiable on the Ethereum chain.

In summary, while eUSD is intended to be a private digital dollar on the MobileCoin network, its deployment includes a manual bridge tied to a Gnosis Safe contract containing a basket of collateral. The Reserve Protocol helps to make eUSD not only the first private digital dollar, but one that is a fully collateralized [US-dollar backed](https://mobilecoin.com/blog/mobilecoin-launches-eusd) stablecoin on Ethereum. This is achieved via a collateralized asset bridge operation where an operator facilitates locking/unlocking and minting/burning on either side of the bridge between MobileCoin and Ethereum. This includes [wrapping](https://medium.com/reserve-currency/introducing-the-electronic-dollar-eusd-cb9d8f50aae) eUSD from Ethereum to MobileCoin and unwrapping eUSD from MobileCoin to Ethereum. A visual diagram of the process is provided below:

![wrapping_eusd](https://github.com/PaulApivat/temp/assets/4058461/3a6a91c9-6a97-498f-9714-286f68693e42)

[source](https://medium.com/reserve-currency/introducing-the-electronic-dollar-eusd-cb9d8f50aae)

With that context, the next section will introduce the Reserve Protocol which facilitates full collateralization and liquidity for eUSD on Ethereum.


# Reserve Protocol Introduction

The Reserve Protocol released their [original white paper](https://reserve.org/assets/files/whitepaper.pdf) in 2018 ([deprecated](https://reserve.org/en/protocol/2018_version/#-version-of-the-reserve-protocol)). Much of the foundation laid in the original paper remains today including the Reserve Dollar (RSV), the protocol’s native stablecoin; the Reserve Rights token (RSR), a token used to facilitate stability and Collateral Tokens, assets to back the RSV. 

Changes that led to its current evolution involves the following:

- The Reserve Dollar (RSV) is primarily used in the [Reserve Mobile App](https://play.google.com/store/apps/details?id=rsv.walletapp.reserve&hl=es&gl=US) for storing value and making payments used by [18,000+ Merchants across Latin American](https://twitter.com/reserveprotocol/status/1524530592741740545?s=20) countries experiencing hyperinflation   
- The Reserve Protocol has evolved into a platform to enable the permissionless [creation and governance of many stablecoins](https://reserve.org/en/about/) (called “RTokens”). 
- The Reserve Rights (RSR) token facilitates the stability of all RTokens created on the platform, not only RSV.

The [current protocol documentation](https://reserve.org/en/protocol/) details how Reserve Protocol currently functions, which forms the basis for this report and subsequent analysis. 

Reserve Protocol allows anyone to create, configure and deploy [overcollateralized](https://reserve.org/en/protocol/) stablecoins (ERC20 tokens called ****RTokens) on Ethereum. They can be pegged to any unit from [US Dollars (i.e., eUSD)](https://register.app/#/overview?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F), to [Ether (i.e., ETH+)](https://register.app/#/overview?token=0xE72B141DF173b999AE7c1aDcbF60Cc9833Ce56a8). RTokens are mintable / redeemable for the [ERC20 collateral basket that they are backed by](https://reserve.org/en/protocol/) at all times, so long as the Reserve Protocol is fully collateralized. 

As Electronic Dollars (eUSD) are unwrapped from MobileCoin to Ethereum, they are deployed as stablecoins (RTokens) on the Reserve Protocol. Because the Reserve Protocol issues asset-backed, yield bearing stablecoins, eUSD is backed by a basket of interest earning stablecoin deposits in lending protocols like Aave and Compound. There are limitations to what can serve as collateral including [non-compatible](https://reserve.org/en/protocol/rtokens/) ERC20 assets like:

- Rebasing tokens
- Tokens that take a fee on transfer
- Tokens that do not expose the decimals() in their interface
- ERC777 tokens which could allow reentrancy attacks
- Tokens with multiple addresses
- Tokens that do not adhere to the ERC20 standard in general

Tokens with any of these limitations will need to be [wrapped into a compatible ERC20](https://reserve.org/en/protocol/rtokens/) to qualify as back-up assets.

In addition to the basket of ERC20 tokens that back each RToken (stablecoin), the protocol enables _overcollateralization_ by allowing its native token, **RSR** (Reserve Rights) to be _staked_ on any RToken ( _overcollateralization_ to that stablecoin.) 

Staked RSR (**stRSR**) can be seized in case of collateral default based on onchain price-feeds that does not rely on governance nor human decision-making. This provides an added layer of protection (“overcollateralization”) to each RToken.

RTokens generate yield from lending out the backing collateral assets and governance for that RToken can _direct_ a portion of revenue to RSR stakers implying that RTokens that generate, and share, revenue are the ones likely to attract stakers and be [protected](https://reserve.org/en/protocol/overall_design/?search=protected#s-result) by overcollateralization, as opposed to RToken governance that choose not to direct revenue to RSR stakers. 

All RTokens launched on Reserve Protocol are governed separately by their respective communities. For example, eUSD (Electronic Dollars) will make governance choices independent of other RTokens like [RSV](https://register.app/#/overview?token=0x196f4727526eA7FB1e17b2071B3d8eAA38486988) (Reserve Protocol’s native stablecoin), [ETH+](https://register.app/#/overview?token=0xE72B141DF173b999AE7c1aDcbF60Cc9833Ce56a8) (an Ethereum-aligned Liquid Staking Token basket), or [hyUSD](https://register.app/#/overview?token=0xaCdf0DBA4B9839b96221a8487e9ca660a48212be) (a decentralized flatcoin that provides access to DeFi yields; [also in process of getting a Curve gauge](https://vote.convexfinance.com/#/proposal/0xbdb05edfcbdeececc5331d1194cae539a366cf37b4269faeb08d8305ea09568f)). Aside from being the [first](https://www.poap.delivery/reserve-eusd) RToken on the Reserve Protocol (not counting its native RSV), Electronic Dollars (eUSD) enjoys the same benefits as other stablecoins launched on the platform. 

The analysis in this article will focus on [eUSD](https://etherscan.io/address/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) and [eusdRSR](https://etherscan.io/address/0x18ba6e33ceb80f077DEb9260c9111e62f21aE7B8), as a specific implementation of RToken and RSR token, respectively, unless the latter are discussed as a general concept. 

**Governance Asset**

Reserve Protocol has a modified version of the [OpenZeppelin Governor](https://docs.openzeppelin.com/contracts/4.x/api/governance) called [Governor Alexios](https://etherscan.io/address/0x7e880d8bD9c9612D6A9759F96aCD23df4A4650E6) which is suggested to all RToken deployers by default to [adjust](https://reserve.org/en/protocol/reserve_rights_rsr/?search=alexios#s-result) governance parameters as they see fit. Governor Alexios allows RSR holders to propose, vote and execute proposals. RSR holders can also delegate their voting power to other addresses. 

Accordingly, Governor Alexios, recommended to eUSD by default, includes the following [configurations](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) as decided by [eusdRSR](https://etherscan.io/address/0x7e880d8bD9c9612D6A9759F96aCD23df4A4650E6#readContract) stakers:

- Proposal threshold (0.01%)
- Quorum (15%)
- Voting Snapshot Delay (2 days, 14,400 blocks)
- Voting Period (3 days, 21,600 blocks)
- Execution delay (3 days)

In addition to their role in governance, eusdRSR stakers receive [100% of revenue distribution](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) as reward for their willingness to provide overcollateralization and be subject to seizure in case the collateral backing for eUSD defaults. For example, [during USDC’s depegging in March 2023](https://medium.com/reserve-currency/eusd-emerges-strong-the-resilience-of-reserve-protocol-during-usdc-depegging-e5a698a990c9), eUSD, which is 50% backed by USDC (approx 25% saUSDC, 25% cUSDC) had to sell off their backing for emergency collateral (USDT) and through the process, eusdRSR stakers helped [re-collateralized eUSD](https://www.poap.delivery/reserve-eusd) in defense of its peg. 

In this instance, eusdRSR stakers provided overcollateralization for eUSD as the USDC portion of the backing collateral defaulted, thus serving as insurance. To compensate, eusdRSR stakers earn rewards based on three factors:

- Amount of revenue eUSD generates
- Portion of revenue governance has directed to eusdRSR stakers
- Portion of total eusdRSR staked on eUSD

For eUSD, [100% of revenue earned is directed](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) to **eusdRSR** stakers. With governance conducted by eusdRSR holders, there is an incentive to keep eUSD safe from unnecessary risk as their funds are literally _at stake_. 

Unstaking **eusdRSR** comes with a [2 weeks](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) [delay](https://etherscan.io/address/0x18ba6e33ceb80f077DEb9260c9111e62f21aE7B8#readProxyContract) as currently decided by governance. This is necessary for staked eusdRSR to remain in the staking contract in case of collateral default so any eusdRSR can be seized to cover losses. During the delay, the staker does not earn rewards to prevent any potential [repeated withdrawing and re-depositing](https://reserve.org/protocol/reserve_rights_rsr/#reserve-rights-staking) interfering with the delay mechanism.  

In addition to providing overcollateralization, governance will also define the basket backing eUSD, as well as a list of emergency collateral. 

Here is the configuration of ERC20 tokens that serve as collateral as proposed and voted by eusdRSR holders:

- Static Aave Interest Bearing USDC ([ssUSDC](https://etherscan.io/address/0x60C384e226b120d93f3e0F4C502957b2B9C32B15)) (25%)
- Static Aave Interest Bearing USDT ([ssUSDT](https://etherscan.io/address/0x21fe646D1Ed0733336F2D4d9b2FE67790a6099D9)) (25%)
- Compound USD Coin ([cUSDC](https://etherscan.io/address/0x39AA39c021dfbaE8faC545936693aC917d5E7563)) (25%)
- Compound USDT ([cUSDT](https://etherscan.io/address/0xf650C3d88D12dB855b8bf7D11Be6C55A4e07dCC9)) (25%)
- Source contracts:[ Basket Handler](https://etherscan.io/address/0x6d309297ddDFeA104A6E89a132e2f05ce3828e07#readProxyContract), [Prime Basket](https://register.app/#/overview?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F).

The Prime Basket defines the collateral needed to be deposited for issuance and it consists of an array of triples: \`**&lt;collateral token, target unit, target amount>**\`. The logic is contained in the [basket handler contract](https://etherscan.io/address/0x6d309297ddDFeA104A6E89a132e2f05ce3828e07#code). For example, the interest bearing Aave USDC token (ssUSDC) is represented in the array as collateral token, target unit and target amount, (i.e., **&lt;ssUSDC, USD, 0.25>**) (see also [bytes32 serialization for USD](https://github.com/reserve-protocol/protocol/blob/master/docs/collateral.md)). In the case where the prime basket is updated by governance due to collateral defaults, the protocol will determine a new reference basket and make changes to the collateral makeup. Finally, the ordered list of [pre-defined emergency collateral](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) consists of pure stablecoins including: USDC, USDT, USDP, TUSD and DAI. 

Electronic Dollars (eUSD) governance (eusdRSR stakers) is in charge of registering, unregistering and swapping ERC20 assets as either collateral or revenue assets. This is achieved through governance to interact with the [Asset Registry contract](https://etherscan.io/address/0x9B85aC04A09c8C813c37de9B3d563C2D3F936162#code), with the following functions:

- Register: Add assets to Asset Registry
- swapRegistered: Allows to modify/update details and functionality of previously registered asset
- Unregister: Removes asset from Asset Registry

With eUSD’s prime basket of collateral defined, anyone can deposit the required collateral tokens (i.e., ssUSDC, ssUSDT, cUSDC, cUSDT) to issue eUSD and conversely, deposit eUSD to redeem the collateral tokens. 

The capitalization and backing of eUSD can be characterized by [two distinct states](https://reserve.org/en/protocol/protocol_operations/?search=redeem#s-result):

- **Fully collateralized**: the [Backing Manager](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4#code) contract holds the right balance of the collateral tokens to offer 100% redeemability (see above for the right mix eUSD’s collateral backing)
- **Fully funded**: there is the right amount of _value_, but not necessarily the right amount of collateral to offer 100% redeemability.

While the Reserve Protocol aims to be fully collateralized at all times, it won’t always be. For example, if governance decides to change the collateral basket or, in cases of market volatility (see USDC depeg scenario above), emergency collateral has to be swapped in as the defaulting collateral is auctioned off, eUSD may be fully funded (right amount of value), but not be fully collateralized (right amount of collateral tokens). Thus, only with full collateralization can a **_redemption mix_** can be made in depositing eUSD for its collateral backing. 

**Stablecoin Peg Mechanisms**

Once deployed, eUSD is designed to trade at $1.00 reflecting the market value of the entire collateral basket while 100% of revenue from earned interest is directed by governance to go towards eusdRSR stakers. Any deviation from $1.00 is designed to get arbitraged away.

This will happen through issuance and redemption mechanisms. The **eUSD RToken contract** has specific functions to regulate the process of issuance and redemption. **Issuance throttle** limits how much eUSD can be issued, to limit value extraction in case of an exploit. After a large issuance, the issuance limit ‘recharges’ to the defined maximum. The **redemption throttle** works similarly where the protocol tries to ensure the net redemption for eUSD never exceeds an hourly limit. The specific parameters for eUSD are as follows: 

- Issuance throttle ([1,000,000 eUSD](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#readProxyContract) maximum amount per the current block) 
- Issuance [throttle rate](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#readProxyContract) at 2.5% of eUSD supply
- Redemption throttle ([1,500,000 eUSD](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#readProxyContract) maximum amount per the current block)
- Redemption [throttle rate](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#readProxyContract) at 5.0% of eUSD supply
- [Source](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)

**Advanced RToken parameters** 

In addition to issuance and redemption throttling, there are other RToken parameters as set in the [Backing Manager](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4#readProxyContract) as part of protocol operations to regulate mint/redeem action including:

- Trading delays(s) which define how many seconds should pass after the basket has been changed before a trade can be opened. For eUSD, this is set by the [Backing Manager](https://etherscan.io/address/0x6d309297ddDFeA104A6E89a132e2f05ce3828e07#code) contract to [2 hours](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) or 7,200 seconds
- Backing buffer (%) as collateral tokens appreciate, eUSD can be minted whenever the correct ratio of collateral tokens is gathered, providing revenue capture. Collateral tokens get sent to the [Revenue Trader](https://etherscan.io/address/0x3d5EbB5399243412c7e895a7AA468c7cD4b1014A#code) contract to mint additional eUSD that can be used as yield for [eusdRSR](https://etherscan.io/address/0x18ba6e33ceb80f077DEb9260c9111e62f21aE7B8) stakers. This is currently set to [0.01%](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) 
- Max trade slippage (%) is the maximum deviation from oracle prices that any trade the protocol can clear. Maximum trade slippage permits additional price movement beyond worst-case oracle pricing. The setting for eUSD is [1%](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) 
- Minimum trade volume represents the smallest amount of value worth executing a trade for; eUSD [minimum trade volume](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) is set to $1,000 
- RToken Maximum trade volume is the maximum sized trade for any trade involving eUSD, currently set to [1,000 eUSD](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)
- Auction Length(s) is determined by the [Broker](https://etherscan.io/address/0x90EB22A31b69C29C34162E0E9278cc0617aA2B50#code) contract and sets how long auctions stay open for. If set too low, arbitrageurs won’t have enough time to complete arbitrage loops; if set too high, fewer auctions will fill. Currently, this is set to [900 seconds](https://etherscan.io/address/0x90EB22A31b69C29C34162E0E9278cc0617aA2B50#readProxyContract) [(15 minutes)](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) for eUSD 

The following section provides case scenarios for when eUSD deviates from peg:

**Scenario 1: eUSD trading below peg**

Assume that eUSD is currently trading at $0.95 cents. An arbitrageur notices the price difference and decides to buy eUSD at the discounted price. The arbitrageur would need to buy at least $1,000 worth of eUSD (minimum trade volume) to execute the trade. However, they must wait for the 2 hour trading delay. Once the delay expires, the arbitrageur can execute the trade with a maximum trade slippage of 1%.

After buying eUSD, the arbitrageur would then redeem it for the basket of collateral tokens backing its peg (see Prime basket). The redemption throttle limits the maximum amount that can be redeemed to 1,500,000 eUSD per block, so the arbitrageur would need to wait until the redemption throttle resets before redeeming more eUSD.

The backing buffer is set at 0.01%, so the arbitrageur can be sure that the collateral backing the eUSD is sufficient to maintain the peg. In case the collateral value falls below the backing buffer of 0.01%, the eUSD issuer can take actions to replenish the collateral and maintain the peg.

Overall, arbitragers would help eUSD regain its peg by buying the stablecoin at a discount, redeeming it for the underlying collateral, and thereby reducing the supply of eUSD in circulation until its price returns to the peg.

**Scenario 2: eUSD trading above peg**

If eUSD is trading above peg at $1.05 cents, arbitragers would take advantage of this price discrepancy by selling eUSD on the market and buying its underlying collateral tokens to redeem eUSD at the peg value of $1.00 USD.

To redeem eUSD at the peg value, arbitragers would first purchase the underlying collateral tokens in the market with the excess eUSD that they hold. They would then deposit these purchased collateral tokens to issue eUSD at the 1:1 peg value. The arbitragers would then sell the eUSD back on the market as long as it is trading above peg ($1.05).

Although the Reserve Protocol intends for arbitragers to help restore peg, the various backing parameters could constrain how fast that happens. For example:

- Trading delay (2 hours) could make it difficult for quickly buy/sell eUSD in response to market changes
- The minimum trade volume of $1,000 could limit the ability of smaller arbitragers to trade and help restore peg
- The RToken maximum trade volume at 1,000 eUSD could make it more difficult for arbitragers to take advantage of small price differences
- The 15 minute auction length duration would limit the amount of time available for arbitragers to participate in the auction and help bring eUSD back to peg.

However, as long as the price discrepancy between eUSD and its underlying collateral tokens persists, arbitragers would continue to exploit the price discrepancy until eUSD returns to its peg value of $1.00 USD.

**Market**

Electronic Dollars (eUSD) trading is currently limited to decentralized exchanges. [Coingecko](https://www.coingecko.com/en/coins/electronic-usd#markets) lists the DEX 4swap with trading pairs eUSD/USDC and eUSD/MOB. Although available on Uniswap, it is not frequently traded. Onchain data suggests most of the trading activity is on Curve, with the following pairs:

- eUSD / USDC
- eUSD / FRAX
- eUSD / crvFRAX

The three pairs have shown up in different pair patterns on DEXes as depicted here:

![eUSD_dex_pairs](https://github.com/PaulApivat/temp/assets/4058461/aad97558-97e9-4e09-b24f-acef0380ddc6)

[Query source](https://dune.com/queries/2452911/4041758)

Date and amount traded between the pairs can be queried in the table below:

![eUSD_dex_trade](https://github.com/PaulApivat/temp/assets/4058461/a1186d17-9233-45eb-90a9-40698da2ae4d)

[Query source](https://dune.com/queries/2458380/4042115)

Given that eUSD only recently had its first transaction on Ethereum as of [February 23, 2023](https://etherscan.io/tx/0x949e6329b55a89b0874ef99256048ffde458bc4f2236e0e87331321a13a8bef8), we can check Coingecko for its price stability. For the most part, eUSD has maintained peg during its short life, with the exception of [March 11th](https://medium.com/reserve-currency/eusd-emerges-strong-the-resilience-of-reserve-protocol-during-usdc-depegging-e5a698a990c9), when USDC depegged bringing nearly all stablecoins in DeFi off their respective peg. Fortunately, [eusdRSR stakers](https://www.poap.delivery/reserve-eusd) were able to help eUSD regain peg as emergency collateral (USDT) was swapped in. 

![eUSD_price_stability](https://github.com/PaulApivat/temp/assets/4058461/d899bf32-ae97-464a-a447-e9e64aaf0391)

[Coingecko: Electronic USD price chart](https://www.coingecko.com/en/coins/electronic-usd)

Examining buy and sell action across DEX (Curve), there has primarily been buying activity for eUSD with a notable uptrend since vote for a [Curve gauge passed on March 23, 2023](https://etherscan.io/tx/0x5708af06b4f1514bc64dcfd9c8cee14c2aa0f5c1c7e2de914d1c2bdfa2726d31).

![eUSD_trade_vol_180d](https://github.com/PaulApivat/temp/assets/4058461/e230f041-cffb-453b-894a-488e86e38932)

[Query source](https://dune.com/queries/2458342/4041981)

Since the first time eUSD was minted for its underlying collateral basket (ssUSDC, ssUSDT, cUSDC, cUSDC) back on [February 23, 2023](https://etherscan.io/tx/0x949e6329b55a89b0874ef99256048ffde458bc4f2236e0e87331321a13a8bef8), eUSD supply has expanded to $13,223,011. A notable uptick in total supply occurred on March 23, 2023 with the passing of the [Curve gauge vote](https://etherscan.io/tx/0x5708af06b4f1514bc64dcfd9c8cee14c2aa0f5c1c7e2de914d1c2bdfa2726d31). 

![eUSD_supply](https://github.com/PaulApivat/temp/assets/4058461/85969d98-1a59-4f1d-94b8-329497577510)

[Query source](https://dune.com/queries/2454760/4035488)

Here are four entities who account for [99% of eUSD supply](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#balances) on Ethereum:

- ~6,154,434 eUSD in [eUSD-3CRV metapool](https://etherscan.io/address/0xaeda92e6a3b1028edc139a4ae56ec881f3064d4f)

- 5,990,000 eUSD in [2-of-4 Multisig](https://etherscan.io/address/0x3c1d46c769f3c09b22c7e9c7144eb392102088b7)

  - [One of the signers](https://etherscan.io/address/0x1871Cf6F04bD5dC3414B6EB0130B09712a6CF2F1) is also on Reserve’s [Slow Wallet Multisig](https://etherscan.io/address/0xA7b123D54BcEc14b4206dAb796982a6d5aaA6770#code)

- 510,002 eUSD in a [custodian multisig Gnosis Safe](https://app.safe.global/balances?safe=eth:0x30DA4EB397215cF407C46854CA7188f4e60F3402)\* ([2-of-2 multisig](https://etherscan.io/address/0x30da4eb397215cf407c46854ca7188f4e60f3402))

  - There’s a [correspondent 510,002 eUSD](https://auditor.mobilecoin.foundation/) on the MobleCoin network 

- ~499,642 eUSD in an [EOA](https://etherscan.io/address/0x79e76c14b3bb6236dfc06d2d7ff219c8b070169c) 

\* This [custodian multisig](https://etherscan.io/address/0x30da4eb397215cf407c46854ca7188f4e60f3402) that holds 510,002 eUSD is itself controlled by 2 additional multisigs. These two additional multisigs are held by a 1-of-3 and 2-of-3 multisigs themselves.  One of the signers in the 2-of-3 multisigs has the address: 

0x1871Cf6F04bD5dC3414B6EB0130B09712a6CF2F1

Which also happens to be a signer on the [Slow Wallet Multisig](https://etherscan.io/address/0xA7b123D54BcEc14b4206dAb796982a6d5aaA6770#readProxyContract) which controls the RSR contract in Reserve Protocol. 

Nearly half of the total supply of eUSD is in the eUSD-3CRV pool with another half controlled by a 2-of-4 multisig, in which one of the signers is also on Reserve’s Slow Wallet multisig ([SlowWallet](https://etherscan.io/address/0x6bab6EB87Aa5a1e4A8310C73bDAAA8A5dAAd81C1#code), [SlowWallet owner - Reserve Rights multisig](https://etherscan.io/address/0xA7b123D54BcEc14b4206dAb796982a6d5aaA6770)) (more on that below).

Finally, we see the largest spike in staked eusdRSR on March 23rd, following the addition of the Curve gauge. For other additional charts showing the state of Electronic Dollars, we have created a [Dune Dashboard](https://dune.com/paulapivat/llama-risk-assessment-electronic-dollar-eusd).

![eusdRSR_staked_ts](https://github.com/PaulApivat/temp/assets/4058461/7a520b58-3168-42b9-b33c-d7020686658b)

[Query source](https://dune.com/queries/2465756/4055736)


# Risk Vectors

(Introduce the relevant risk vectors. Typical vectors listed as sections below, but modify to suit the findings from your research.)


## Smart Contract Risk

[Trail of Bits](https://github.com/reserve-protocol/protocol/blob/master/audits/Trail%20of%20Bits%20-%20Reserve%20Org%20-%20Final%20Report.pdf) submitted a security assessment on August 11, 2022. The audit found five high, two medium, three low severity and five informational issues. The high severity issues are summarized in the table below. The team has taken action to mitigate severity around Access Control (see Centralization Risk), but it is unclear how the other four high severity issues were addressed. 

|                                                                                                                                                                                                                            |              |                 |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- |
| **Description**                                                                                                                                                                                                            | **Severity** | **Type**        |
| Lack of a two-step process for contract ownership changes                                                                                                                                                                  | High         | Data Validation |
| All auction initiation attempts may fail (see [EasyAuction](https://etherscan.io/address/0x0b7fFc1f4AD541A4Ed16b40D8c37f0929158D101#code) contract)                                                                        | High         | Data Validation |
| All attempts to initiate auction of defaulted collateral tokens will fail (affects Recapitalization strategy and [Backing Manager](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4#code) contract) | High         | Data Validation |
| An RSR seizure could leave the stRSR contract unusable                                                                                                                                                                     | High         | Data Validation |
| System owner has excessive privileges. The owner of the [Main](https://etherscan.io/address/0x7697aE4dEf3C3Cd52493Ba3a6F57fc6d8c59108a#code) contract has excessive privileges (see next section for mitigation actions).  | High         | Access Control  |

A Code4Arena audit was conducted in April, 2023 and [two high and twenty seven medium severity](https://github.com/reserve-protocol/protocol/blob/master/audits/Code4rena%20Reserve%20Audit%20Report.md) issues were found. A table summarizing the status of the two high severity issues is provided here:

|                                                                                                                                              |              |                                    |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ---------------------------------- |
| **Description**                                                                                                                              | **Severity** | **Status**                         |
| Adversaries can abuse a quirk of Compound redemption to manipulate the underlying exchange rate and maliciously disable cToken collaterals.  | High         | Mitigation confirmed with comments |
| Basket range formula is inefficient, leading to unnecessary haircuts for the protocol.                                                       | High         | Not fully mitigated.               |

[Ackee Blockchain](https://github.com/reserve-protocol/protocol/blob/master/audits/Ackee%20-%20abch-reserve-protocol-report-1.1.pdf) also provided an audited report to the Reserve Protocol on October 7, 2022 finding three medium issues and six warnings. The issues have either been acknowledged or fixed. 

The Reserve Protocol has been audited by [Solidified](https://github.com/reserve-protocol/rsr-mainnet/blob/master/audits/solidified/Audit%20Report%20-%20Reserve%20Token%20%5B3%20Jan%202022%5D-2.pdf) as of January 4, 2022. There were no critical, major or minor issues found. The one issue around missing input address validation had been resolved. While code complexity was at a medium level, code readability, documentation and test coverage were all high. 

[Halborn](https://github.com/reserve-protocol/protocol/blob/master/audits/Halborn%20-%20Reserve_Protocol_Smart_Contract_Security_Audit_Report_Halborn_Final.pdf) also conducted a Smart Contract Security Audit from August 28th, 2022 - October 10th, 2022 and did not find any critical flaws in the protocol. Any security risks found were mostly addressed by the Reserve team. 

Finally, there is an on-going bug bounty program by [Immunefi that has been live since April 27, 2023](https://immunefi.com/bounty/reserve/). 

One potential issue that could arise in the future is the upgradability of the contracts with the [Owner](https://reserve.org/protocol/system_states_roles/) role having the ability to upgrade system contracts. Additional audits should be taken if and when there are future contract upgrades. 

  
In summary, the Reserve Protocol team has subjected themselves to several rounds of audits. Two of the auditors found several high severity issues. **It is recommended the team takes steps to publicly report their progress in addressing these issues**.

**Centralization Risk**

The [Main contract](https://etherscan.io/address/0x7697aE4dEf3C3Cd52493Ba3a6F57fc6d8c59108a#code) is central to the functioning of [eUSD](https://etherscan.io/address/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#code) (as [MainP1](https://etherscan.io/address/0x143C35bFe04720394eBd18AbECa83eA9D8BEdE2F#code), which Main is a proxy for is central to the Reserve Protocol). The Main contract is linked to other contracts essential to various protocol operations including: Asset Registry, Backing Manager, Basket Handler, eUSD, eusdRSR, Broker, Furnace, Distributor, RToken Trader and RSR Trader (see [Access Control sheet](https://docs.google.com/spreadsheets/d/1_tdf1WDr6QMAIVZcEJpMb1Pc43b0W5qRZeqq0ByeH6Y/edit?usp=sharing)). 

There are certain contracts that have ownership and/or permissions over Main, shown below. Reserve Protocol has attempted to mitigate potential centralization risk by spreading out permissions over Main to two externally owned wallets (EOAs), two multisig wallets and the TimelockController contract.

![Permissions_over_Main](https://github.com/PaulApivat/temp/assets/4058461/99dd225f-a4de-40bc-9176-7a055d223297)

[TimelockController](https://etherscan.io/address/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c#code) holds a host of [permissions on Main](https://pod.xyz/podarchy/0x7697aE4dEf3C3Cd52493Ba3a6F57fc6d8c59108a?lens=permissions&node=0x7697aE4dEf3C3Cd52493Ba3a6F57fc6d8c59108a) and plays a high authority role such as RToken [Pauser, Short Freeze and Long Freeze](https://reserve.org/en/protocol/system_states_roles/) ([source](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)). These are core [system states and roles](https://reserve.org/en/protocol/system_states_roles/) in Reserve Protocol’s RToken governance system. 

- RToken Pauser: can pause and unpause an RToken’s (i.e., eUSD) system in case of an emergency such as a Chainlink feed failing. False positives are acceptable as redemption remains enabled. There can be multiple Pausers and it can be robot-controlled. Pausing means RToken issuance, un-staking RSR, withdrawing RSR, trading and RToken melting are disabled 
- Short Freeze: can freeze an RToken’s system for three days, generally assigned to an entity that can spot bugs and react swiftly. There is some tolerance for false positives, though less than the Pauser role. There can be multiple Short Freezer, as is the case with [eUSD](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F), as well as robot-controlled entities. 
- Long Freeze: can freeze an RToken’s system for one week. This role should be optimized for no false positives, and is expected to act slowly and surely. There are fewer Long Freezers, with TimelockController being one of two for eUSD.

In an attempt to decentralize, it appears the eUSD team has assigned two entities (Alexios Governor and a Multisig_1_of_1), [permission over TimelockController](https://pod.xyz/podarchy/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c) as shown here:

![Permissions_over_TimelockController](https://github.com/PaulApivat/temp/assets/4058461/d053e783-7a6f-44b1-9aae-fa1adf3aa8f2)

This implies that the OWNER role in the eUSD RToken system is shared between a governance contract and a multisig, as intended by the Reserve Protocol design:

- Owner: top decision maker, typically assigned to a decentralized governance smart contract, the [Governor Alexios](https://etherscan.io/address/0x7e880d8bD9c9612D6A9759F96aCD23df4A4650E6) contract for eUSD. This role has the power to grant and revolve roles to any Ethereum account, pause and unpause the system, freeze and unfreeze the [system](https://reserve.org/en/protocol/system_states_roles/), set governance parameters and upgrade system contracts. 

The Governor Alexios contract, a modified version of [OpenZeppelin Governor](https://docs.openzeppelin.com/contracts/4.x/api/governance), allows [eusdRSR](https://etherscan.io/address/0x18ba6e33ceb80f077DEb9260c9111e62f21aE7B8) holders to propose, vote and execute proposals. The TimelockController mediates this process by introducing a timelock once a proposal is approved, adding a delay between approval and execution, giving RToken holders to make a decision before something is changed. The process of approving to execute is 8 days (i.e., [voting snapshot delay: 2 days, voting period: 3 days, execution delay: 3 days](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)). 

Although Governor Alexios allows eusdRSR holders to directly participate in governance, it is unclear the nature of the relationship between the Governor contract and the [1-of-1 Multisig](https://etherscan.io/address/0x576ca79e46171a2B3E26F13a4334940eBcD72164#code) that shares [power over the TimelockController](https://pod.xyz/podarchy/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c) ([Access Control](https://docs.google.com/spreadsheets/d/1_tdf1WDr6QMAIVZcEJpMb1Pc43b0W5qRZeqq0ByeH6Y/edit?usp=sharing)). Having one address potentially wield significant influence over the entire system, presumably constrained by eusdRSR holder, is nevertheless a _potential_ centralization risk. 

On the other hand, it is the opinion of this author that the Reserve Protocol team has taken step to address this potential centralization risk by distributing permissions over Main, RToken_Pauser, Short_Freeze and Long_Freeze roles, across an additional four externally owned addresses (EOAs) and an additional two multisig natures wallets as depicted below (see [Access Control sheet](https://docs.google.com/spreadsheets/d/1_tdf1WDr6QMAIVZcEJpMb1Pc43b0W5qRZeqq0ByeH6Y/edit?usp=sharing)).

In summary, while certain addresses have significant privileges and control over protocol operations and governance, there has been an attempt to mitigate potential centralization risk with a diversity of non-overlapping externally owned accounts (EOAs) and multisig wallets serving as a check and balance on power. Should any of the multisig signers disappear, there is sufficient distribution of permissions such that the protocol could continue functioning. The only entity that requires only one signature to exercise extensive power is the [1-of-1 multisig](https://etherscan.io/address/0x576ca79e46171a2B3E26F13a4334940eBcD72164#code) with permissions over the TimelockController contract. However, that is constrained by the power held by eusdRSR token holders through governance. **Nevertheless, it is recommended the team clarify whether that 1-of-1 multisig has the ability to veto proposals submitted by governance**.

![mitigate_centralization_risk](https://github.com/PaulApivat/temp/assets/4058461/4d87cc85-d659-4035-87a5-8b8ab357e7ce)

## Collateral Risk

The Electronic Dollar (eUSD) is 100% backed by a basket of yield bearing stablecoins (cUSDC, cUSDT, ssUSDC, ssUSDT) with [emergency](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) [collateral](https://etherscan.io/address/0x6d309297ddDFeA104A6E89a132e2f05ce3828e07#code) of pure stablecoins (USDC, USDT, USDP, TUSD, DAI). 

Additionally, eUSD is _overcollateralized_ with staked RSR (eusdRSR). As of this writing $13,223,011 in eUSD is backed by a collective prime basket and staked RSR valued at $15,292,097.

-  $13,223,011 (eUSD)

- Prime basket

  - $2,380,019 (cUSDT)
  - $3,125,261 (cUSDC)
  - $3,047,063 (saUSDC)\*
  - $2,974,599 (saUSDT)\*
  - $11,526,942 (Total Prime Basket value)

- Staked RSR

  - $3,765,155

- Total Backing

  - $15,292,097

This overcollateralization does not account for the emergency collateral. In that case, [staked eusdRSR](https://www.poap.delivery/reserve-eusd) and emergency collateral could serve as backstop.

Nevertheless, as identified in the audit report from [Code4Arena](https://github.com/reserve-protocol/protocol/blob/master/audits/Code4rena%20Reserve%20Audit%20Report.md), there is a quirk in Compound that could maliciously disable cToken collaterals in eUSD (currently: cUSDC and cUSDT). Therefore, there is collateral risk from exposure to external protocols like Compound and Aave. 

\*Note: The values for Static Aave Interest Bearing USDC (saUSDC) and Static Aave Interest Bearing USDT (saUSDT) use USDC and USDT dollar values respectively as Dune Analytics currently does not make prices for saUSDC and saUSDT available.

![eUSD_overcollateralized_May25_2023](https://github.com/PaulApivat/temp/assets/4058461/d6fda070-b950-4fb7-ba6d-bb7ae7832a24)

[eUSD overcollateralization: May 25, 2023](https://dune.com/paulapivat/llama-risk-assessment-electronic-dollar-eusd)


## 


## 


## 


## Custody Risk

The prime basket collateral backing Electronic Dollars (eUSD) is handled by the [backing manager contract](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4#code). This contract sits within the Main contract, which itself is controlled by a diversity of entities who have permission over the system including the TimelockController, two EOAs and two Multisigs (see Centralization section). The Reserve Protocol team has taken steps to mitigate potential custody risk. 


## Oracle Risk

The Reserve Protocol uses only [one Chainlink oracle](https://github.com/reserve-protocol/protocol/blob/master/contracts/plugins/assets/OracleLib.sol) price feed with no apparent backup. The Backing Manager contract has [maxTradeSlippage](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4#code), setting maximum deviation from oracle prices that a trade can clear at. Moreover, the protocol has the pause, short and long freeze functions to mitigate any extensive oracle failure situation. 



## Depeg Risk

The Electronic Dollar (eUSD) is an overcollateralized stablecoin. While eUSD is designed to trade at the market value of the entire prime basket, it is overcollateralized through the staking of eusdRSR. This provides a level of issuance in case of collateral default as we saw when [USDC depegged in March 2023](https://medium.com/reserve-currency/eusd-emerges-strong-the-resilience-of-reserve-protocol-during-usdc-depegging-e5a698a990c9) and eusdRSR [stakers help re-collateralized](https://www.poap.delivery/reserve-eusd) eUSD to defend its peg. 

Anyone can bring an amount of collateral token value and [mint a corresponding value in RTokens](https://reserve.org/protocol/protocol_operations/) (eUSD) in exchange. The eUSD community currently has set an issuance throttle at (1,000,000 eUSD) and a redemption throttle at (1,500,000 eUSD) to [regulate mint and redeem](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) supply and prevent attacks on the eUSD stablecoin. Moreover, governance has other tools such as pausing and freezing at its disposal to ensure normal protocol functioning. 

In summary, there are sufficient reserves from overcollateralization. There is no reliance on an external AMM as the mint and redeem operations are built into the protocol. Stability is maintained via market arbitrage, while bribery mechanisms via the recent voting for Curve gauge will be used to incentivize liquidity. While there is low risk of depeg at the moment, eUSD is relatively young and further monitoring of the peg is warranted as the stablecoin continues to increase in market cap.

**LlamaRisk Gauge Criteria**

(An analysis of all the above findings are used to answer the following questions, which we ultimately use to determine whether we recommend a gauge or not)

**Centralization Factors**

1. Is it possible for a single entity to rug its users?

The team has put in place a diverse set of externally owned accounts, multisigs and contracts that share permission over crucial functioning of the protocol, including the collateral in the backing manager. Although there is one entity with extensive privileges (see 1-of-1 multisig above), that influence is constrained by governance making it difficult for any single entity to rug users. 

2. If the team vanishes, can the project continue?

The Reserve Protocol platform is designed such that anyone can permissionlessly create a new RToken through a system of factory smart contracts such that the Electronic Dollars (eUSD) project on ethereum can continue.

On the MobileCoin front, operations are more centralized and their functioning could be more reliant on the core team.

**Economic Factors**

1. Does the project's viability depend on additional incentives?

The project is currently viable without incentives, but a deeper Curve pool that combines eUSD with other stablecoins could further strengthen its peg.  

2. If demand falls to 0 tomorrow, can all users be made whole?

Electronic Dollars (eUSD) is currently sufficiently overcollateralized such that users can be made whole if demand fell to 0.

**Security Factors**

1. Do audits reveal any concerning signs?

The audits did not reveal any critical, major or minor issues to be concerned with. Although the code and protocol design is fairly complex, the team has made efforts to ensure high readability and clarity in their documentation and test coverage. 


# Risk Team Recommendation

(Don't worry about this section in the first draft, we will discuss together and with the protocol team to determine our final recommendation)

