    BSIP: 00XX
    Title: Sharedrop operation
    Authors: OpenLedgerApp <https://github.com/OpenLedgerApp>
    Status: Draft
    Type: Protocol
    Created: 2018-12-14
    Updated: 2018-12-14
    Discussion: 
    Worker: 
 
A sharedrop is a giveaway of some tokens to a certain group of people, for example to all BTS holders in proportion to their holdings.
# Abstract
We should add the ability to distribute arbitrary amount of some token (SHAREDROP token) to holders of a specific other token (STAKE token)  according to the share(stake) of such tokens.
The importance of a similar solution was raised in BSIPs-19, 20,  however never implemented. (link below).


# Motivation
TBD - девелоперам не нужно
# Rationale
TBD - девелоперам не нужно


- Other cryptocurrency platforms offer profit-sharing/dividends functionality, such as Peerplays/NXT/CounterParty/DigixDAO/LBRY/Waves/Dash.
- Most (if not all) Bitcoin forks (1000's of them) have 'sendmany' functionality which enables the mass distribution of their tokens against a large quantity of users (addresses) in a single transaction; this is currently not possible within the BTS DEX (unless I'm mistaken?).

To make it more difficult to abuse the system dropping multiple useless tokens, there will be a vesting stake for the sharedrop operation. For example, vest 5000 bitUSD (in BTS) for 2 month to be able to do sharedrop operation. 

Sharedrop operation should also be quite expensive as well. 
However, to make such an operation more lucrative for BtiShares - the sharedrop operation will send the money to each stakeholder's vesting balance. Thus making them withdraw the drop from the vesting balances.

 
# Specifications
## General workflow
First of all, there is a need to calculate transaction fee, so **dry_run_sharedrop** operation should be called on a local node to do calculations.  THe results will be provided (displayed) to a user. 

Then, knowing that user has enough BTS for transaction fees and vesting money, he should call Sharedrop operation itself. Vesting specifics will be covered below. 

Sharedrop operation will drop the Sharedrop asset in proportion to the stake that stakeholders have to their respective vesting balances.

## dry_run_sharedrop operation 
This is a locally done operation with the following parameters: 

- Sharedrop asset
- Sharedrop amount
- Stake asset
- Minimum stake asset amount to participate in airdrop (**minimum_amount_for_drop**)
dry_run_sharedrop operation returns transaction fee for the sharedrop operation as well as if a user has enough money in his sharedrop vesting or if he should prepare additonal funds.

This will be approximate calculation as the final calculation will be determined only during the sharedrop operation.

The following parameters are calculated by a node and presented to the user:  

- Sharedrop operation price
- Sharedrop vesting stake 
- Sharedrop vesting period

## Sharedrop operation
Sharedrop operation should be available with the following parameters:

- Sharedrop asset
- Sharedrop amount
- Stake asset
Minimum stake asset amount to participate in airdrop (**minimum_amount_for_drop**)
Sharedrop operation will drop the Sharedrop asset in proportion to the stake that stakeholders have to their respective vesting balances.

Each stakeholder will then need to retrieve the sharedrop token from the vesting balance in a separate operation.

If the amount to distribute for stakeholders is too small and cannot be distributed among stakehoders or part of stakeholders then it accumulates and is returned to the account that originated sharedrop operation.

## Sharedrop Parameters 

### Minimum amount to participate in sharedrop.
If an asset holder has too little amount of the stake token it becomes expensive for the blockchain to do sharedrop for such a user.
So a sharedrop operation should have **minimum_amount_for_drop** property. 

### Sharedrop price
sharedrop price should be determined based on the number of accounts (stakeholders) to split the value for, as in `stakeholder_number * sharedrop_price_multiplier`, where **sharedrop_price_multiplier** is defined by the committee.

## Sharedrop vesting stake 
Sharedrop vesting stake will be a flat fee determined by the committee. It will be charged in BTS.

It will be in BTS only and it will be held on a vesting balacne under a specific sharedrop vesting policy  for the period of **sharedrop_vesting_period (in weeks)** defined by the committee. At the end of the sharedrop_vesting_period, a sharedrop originator will be able to retrieve his funds from the vesting account. 

Let's say a vesting period is 2 months.
If a sharedrop originator does another sharedrop before the vesting period is over, the **sharedrop_vesting_period** counter resets and these 2 months will start from fresh.

### Complicated vesting cases, when the committee updates sharedrop vesting parameters.
- Vesting stake increases. In this case **dry_run_sharedrop** operation warns the user that he should have more money prepared. And then the sharedrop operation will put more money into vesting. 

- Vesting stake decreases. Then the extra money becomes instantly available for withdrawal.  Balance availability is checked during vesting withdrawal operation.

-  **Sharedrop_vesting_period** decreases or increases. The new vesting period is updated during the next sharedrop operation. 

## Technical specifications
```
struct sharedrop_result {
	// cost of sharedrop
	share_type fee;
	// required amount on the vesting balance
	share_type vesting_stake;
	// money may be claimed after this time
	fc::time_point_sec vesting_period;
}
```


There is a function that helps to fill parameters for sharedrop operation

```
// function that helps to fill parameters for sharedrop operation
// returns  sharedrop_result
sharedrop_result dry_run_sharedrop(asset sharedrop_asset, asset_id_type stake_asset,
share_type min_stake_asset_amount) 
```

For sharedrop vesting balance add specific type for vesting_balance_object

```
struct sharedrop_vesting_policy
{
	// money may be claimed after this time  
	fc::time_point_sec  start_claim;
}
```

After each sharedrop operation start_clam will be updates as start_claim =TODAY + sharedrop_vesting_period


# Discussion
1. Flag **allow_sharedrop** for an asset. Should we have it?
Each asset should have **allow_sharedrop** flag (TRUE/FALSE) that would either allow sharedrop to the holders of this asset or not allow it.

2. One of the ideas is instead of making flat fee vesting for all sharedrop transactions, for each sharedrop transaction we put into vesting an amount based on the stakeholder number. And we have to put a new amount for every sharedrop transaction.
In this case, it might quickly become too expensive for some users.


# See Also

https://github.com/bitshares/bsips/blob/master/bsip-0020.md

## Copyright
This document is placed in the public domain.

# CORE TEAM TASK LIST
- [ ] Evaluate / Prioritize Feature Request
- [ ] Refine User Stories / Requirements
- [ ] Define Test Cases
- [ ] Design / Develop Solution
- [ ] Perform QA/Testing
- [ ] Update Documentation



