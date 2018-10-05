    BSIP: 0043
    Title: Market fee sharing
    Authors: OpenLedgerApp <https://github.com/OpenLedgerApp>
    Status: Draft
    Type: Protocol
    Created: 2018-08-23
    Updated: 2018-10-05
    Discussion: https://github.com/bitshares/bsips/issues/102
    Worker:


# Abstract

When creating a new asset, the asset owner is the only beneficiary of the market fees in the current implementation. And one of the ways to increase the community growth is the market fee percentage. For example, one can decrease the market fee and it will result in somewhat larger number of trades with this asset. In this way the asset owner might get a bigger profit during to increasing the trade volume with this asset.

However, there might be another opportunity to promote the asset and stimulate the trading - use native Bitshares referral program. At this time unfortunately an assets owner is not able to share market fees with registrars and referrers to stimulate them to market the asset trading, so we suggest to add this possibility. Furthermore, enabling this feature for MPAs (e.g. bitCNY or bitUSD) can provide additional bounty for Bitshares registrars and referrals which can lead to more traders joining to the ecosystem.

The workflow may be as follows. It is copied from vesting balance mechanism and got simplified.

An asset owner defines the **market_fee_reward_percent**  in asset options - what percentage of the market fee he wants to share with the registrar. 
market_fee* market_fee_reward_percent goes the registar's fee pool.
market_fee*(1 - market_fee_reward_percent) goes to the asset owner. 


Registrar defines **reward_percent** - for the referrer for each user(using the already existing BitShares mechanism). 

Market fee reward is accumulated on the user account. 

Each user decides when they want to claim the market fee reward and move it to their active balance. 
There is another operation created asset_claim_reward_operation for this. 
And each user pays network fee to call this operation.

# Motivation

To make promoting BitShares and bringing new users much more attractive to registrars and referrers by sharing UIAs and MPAs market fees with them.

# Rationale
When 'fill_order_operation' executing at the moment of market fee calculation there will be calculate the reward for the registrar used the parameter market_fee_reward_percent. Then this market_fee_reward_percent of the market fee is split between the registrar and referrer according to the referrer_rewards_percentage, which is set up during the new account registration by registrar (please see the parameters for register_account).

# Specifications

##Design decisions.
There has been a discussion going on about which way to implement distribution of the market fees for referrers.
Direct transfer and using vesting balance mechanism are some of the alternatives.
In our approach there has been decided to repicate the current functionality (to use the current vesting balance mechanism).
There is also an option to simplify the current design decisions specified below.
However, it requires that the related bitshares/bitshares-core#1276 is already implemented. If #1276 is implemented before BSIP-43, than BSIP-43 can be changed to accomodate  #1276 changes.


## market_fee_reward_percent asset option
Percent of market fee that will be paid to buyer's registrar. Set by asset owner.
if market_fee_reward_percent = 0 or absent - the old mechanism is used. Market fees are accumulated in the asset and can be claimed.

## market_sharing_whitelist asset option
An optional list of accounts (configurable by the asset owner) could provide increased control over who is eligible to get market rewards.
If whitelist empty or absent - there is no filtering. This means that everyone is in the whitelist by default if it is empty.

## New database and wallet api methods
**get_account_reward(account, asset_symbol)** - Returns information about the reward amount for specific asset.
**list_account_rewards(account)** - Returns information about the reward amount in various assets
**claim_reward(account, asset_symbol, amount)** - Claim account's reward for the given asset.

## account_reward_object
A new BitShares object type in implementation space impl_account_reward_object_type = 2.18.X that tracks the rewards of a single account/asset pair.
Usually, vesting_balance_object receives fees from all kinds of transactions.
In this case, a user can't find out how much profit is received from market fee sharing. So a new object account_reward_object is created in order to track profits from market fee sharing only.
```
account_reward_object {
owner_account,
asset_type,
reward_amount
}
```
## account_reward_index
A new index that stores objects of account_reward_object-type in graphene::database and allow random fast access to objects by given criteria
```
account_reward_index multi-index of account_reward_objects
indexed_by
[id]
[owner_account, asset_type]
[asset_type, reward_amount desc, owner_account]
```
## asset_claim_reward_operation
A new operation used to transfer reward to the account's balance.
The purpose of this operation is to track profits from market fee sharing.
```
asset_claim_reward_operation {
fee
claiming_account
amount_to_claim
}
```

## graphene::chain::database new methods
**get_reward(owner_account, asset_id)** - Retrieve a particular account's reward in a given asset
**adjust_reward(account, delta)** - Adjust a particular account's reward in a given asset by a delta

## graphene::chain::database modifications
**pay_market_fees(seller, recv_asset_object, receives_amount)** - Split pay to asset owner fee, registrar fee and referrer fee. If the registrar is not in the market_sharing_whitelist, split_pay will not happen and the entire fee goes to Asset owner.
**calculate_market_fee(trade_asset, trade_amount)** - Calculate value for previous function.
**fill_limit_order(order, pays, receives, cull_if_small, fill_price, is_maker)** - Append hardfork (HARDFORK_REWARD_TIME) check. Use old or new version of pay_market_fees() function.

## asset_create_evaluator, asset_update_evaluator modifications
Append hardfork (HARDFORK_REWARD_TIME) check. Validate additional asset options. Apply additional asset options (market_fee_reward_percent, market_sharing_whitelist)

## Unit tests

Add: reward_tests.cpp (asset_rewards_test, asset_claim_reward_test)

Modified: fee_tests.cpp, uia_tests.cpp

# Description

# Summary for Shareholders
The new modification - market fee sharing - will allow to bring in new clients for BitShares by making it financially lucrative for registrars and referrers. This modification is interesting mostly so asset owners and registrars/referrers.

# Copyright
This document is placed in the public domain.

# See Also
https://github.com/bitshares/bitshares-core/issues/1268