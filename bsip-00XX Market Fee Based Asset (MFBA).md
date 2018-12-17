    BSIP: TBD
    Title: Market Fee Backed Asset (MFBA)
    Authors: OpenLedgerApp <https://github.com/OpenLedgerApp>
    Status: Draft
    Type: Protocol
    Created: 2018-12-17
    Updated: 2018-12-17
    Discussion: 
    Worker: 

 
# Abstract
Market Fee Backed Asset (MFBA) is a SmartCoin introduced by this BSIP. 
The main idea of a MFBA is to provide opportunity for the Assets Owners to automatically share revenue received from trading operations (Market Fees) to the holders of the MFBA in proportion of the owned amount of the MFBA.

The importance of a similar solution was raised in BSIPs-19, 20,  however never implemented. (link below).


# Motivation
By implementing this proposals, we let Asset Owners an opportunity to use their assets as profit-sharing assets.
Distribution of fees will be conducted automatically (in a pre-defined period of time). Such functionality is currently not available in the blockchain.

# Use case

 

There will be a new type of asset created - Market Fee Backed Asset (MFBA). 

Let's say a user (owner of XXX generic asset) wants to share the market fee from assets  XXX.BTC, XXX.ETH, XXX.YYY with other people. 
Then this user creates an MFBA and calls it XXX.MFBA. Then he ties his other assets: XXX.BTC, XXX.ETH, XXX.YYY to this newly created XXX.MFBA.
Then holders of the XXX.MFBA will get a share of profits from the market fees of all the assets tied to XXX.MFBA.

# Specifications

There will be a special structure for MFBA parameters storing **MFBA_options**. It will be a part of general asset_options

## Market Fee Backed Asset Definition

Asset for which **MFBA_options** structure is available will be called MFBA Asset.

## Tied assets
Issuer of the MFBA Asset will be able to tie his/her other assets (one or many) to the MFBA Asset. Tied assets cannot become MFBA. 

The list of the tied assets will be stored in the **MFBA_options**

## Dividends

Market Fee, received from trading operations of the tied assets will be distributed between holders of the MFBA Asset according to the stake of the MFBA Asset that holders have. 
Issuer of the MFBA Asset must be able to set share of the Market Fee, that will be distributed to the holders of the MFBA Asset as a parameter **(MFBA share, %)**.
Whatever is left after distribution to the MFBA holders, will be the share of the Asset Owner **(100% - MFBA share%)**.
Funds currently in the open orders will not be included in the MFBA share.

**MFBA share, %** will be stored in **MFBA_options**.

## Whitelist and Blacklist

Whitelist and blacklist (configurable by the UIA Issuer) could provide control over who is eligible to earn dividends. 

These lists will be stored in the **MFBA_options**.

## Dividends Distribution Process

Distribution will be performed regularly.  **fee_accumulation_period** is stored in  the **MFBA_options** and must be set by the MFBA Issuer (minimal available interval must exist).
Market Fee of the tied assets is accumulated during this period. At the end of the period fee is distributed to the MFBA holders' **vesting balance** and to the MFBA Issuer **vesting balance**. 
After that MFBA holders and MFBA Issuer can withdraw the fees.

The time of the last MFBA fees distribution for this asset is stored in  the **MFBA_options** as well.

The distribution occurs in the nearest maintenance interval after the  ``latest MFBA distribution time + fee_accumulation_period.

## Dividends Distribution Process Restrictions
Distribution must be performed starting from the user, that has the biggest share of the MFBA Asset.
Users will not receive their share of Market Fee if the amount to be transferred is less than should be accrued, .  
The undistributed market fee will be returned to the asset issuer.

# Discussion

 

# Copyright
This document is placed in the public domain.

# See Also
https://github.com/bitshares/bsips/blob/master/bsip-0020.md

 

# CORE TEAM TASK LIST
- [ ] Evaluate / Prioritize Feature Request
- [ ] Refine User Stories / Requirements
- [ ] Define Test Cases
- [ ] Design / Develop Solution
- [ ] Perform QA/Testing
- [ ] Update Documentation