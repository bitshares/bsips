    BSIP: 00XX
    Title: Dynamic Market Fees
    Authors: OpenLedgerApp <https://github.com/OpenLedgerApp>
    Status: Draft
    Type: Protocol
    Created: 2018-12-10
    Updated: 2018-12-10
    Discussion: https://github.com/bitshares/bsips/issues/130
    Worker: <Id of worker proposal> (optional)
# Abstract
UIA owners do not have many marketing tools to increase trade volume of their assets.

One of the ways to increase the trade volume can be Dynamic Market Fees that will provide additional discounts for active traders.
For example, to set lower fees for trading larger volumes.

So, instead of market fee, an asset owner can optionally set up dynamic_market_fee_percent property that may look like a following example table.
**Please refer to the general notes below for terms explanation.**

Trade daily volume | Maker fee, % | Taker fee, %
< 500 USD | 0.19% | 0.20%
>=500 USD | 0.18% | 0.20%                        

Number             | Title                                                    | Owner             | Type           | Status
-------------------|----------------------------------------------------------|-------------------|----------------|--------
[1](bsip-0001.md)  | BSIP Purpose and Guidelines                              | Fabian Schuh      | Informational  | Draft
[2](bsip-0002.md)  | Refund Create Order Fees on Cancel                       | Daniel Larimer    | Protocol       | Installed

## Motivation
- To provide additional marketing tools to encourage traders. 
- To increase trading volume on BitShares. 
- To attract traders to the ecosystem.

# Rationale

## Use cases:
**1) Supporting market makers.**
Every trade occurs between two parties: the maker, whose order exists on the order book prior to the trade, and the taker, who places the order that matches (or "takes") the maker's order.

Makers are so named because their orders make the liquidity in a market. Takers are the ones who remove this liquidity by matching makers' orders with their own.

The maker-taker model encourages market liquidity by rewarding the makers of that liquidity with a fee discount. It also results in a tighter market spread due to the increased incentive for makers to outbid each other. The higher fee that the taker pays is usually offset by the better prices this tighter spread provides.

**2) Supporting heavy volume traders.**
Fees are charged and deduced on a per-trade basis. The bigger “trading volume” users have traded, the lower fee they get on subsequent trades.

# General notes
Each asset will have either usual market fee or a table with dynamic market fees that will represent the table above. 

A switch **market_fee_type (REGULAR, DYNAMIC)** should clearly indicate, which market fee type is used.

**Makers** are the ones who put up their orders first.

And **takers** are the ones to put up their orders later in time than makers. 

The following table 
# Discussion

We have had a discussion what's the best way to average out Trading Volume.

There are a few ways of doing this: 

- Use an **average volume since day the beginning of trading**.  
Up for discussion is - what to take as a day 1. 1st trade of this asset? 1st trade minus 30 days?  Account open date? 
This way we have a normal fair average volume if we figure out the fair beginning interval date.
However, if a trader started trading this asset a few years ago he might be in a significant disadvantage.

- **Average trading volume over the last 30 days - sliding window**.
This way it is a bit clearer how to calculate. 
However there might be extreme but real cases where this kind of calculation might not be fair: 
a. when a person who usually trades high volumes throughout the year goes to vacation for 30 days. Suddenly all his discount is gone when he comes back
b.   When another person comes in and makes 1 or 2 high volume trades while trading really low volumes before that. Suddenly this person might have a big discount. 
30-day volume 
This mehod gives enough information to figure out last 30-days traders habits with much lower memory requirements but slightly less precision than the 30-day average sliding window.
Average trading volume over the last 365 days (a year) is a variation of this method.

- **30-day volume method**
This is quite similar to the 30-day average method and shares it's advantages and drawbacks.

# Technical Specifications

We suggest using average total volume for a particular asset. However, it's open for the discussion.

## Statistics Storage

There are two options for storing account statistics for trading:

1. expand class account_statistics_object by adding a map of trade_statistics_object for each asset
2. create a new type trade_statistics_object and a new index


## Statistics Update
Statistics updates during fill_order. 
We need to update statistics for the user and for the specific asset at the same time.

 

## Calculations

### Average since Day 1

**Trader Activity** is estimated as average trading volume of a user for a given asset for 1 day.

`Trader_activity = total_volume / (Now - first_start_date) //(in days).`

This way we accumulate total bought volume for each user for each asset and average out daily volumes for his entire term of trading of this asset (per day).

Volume discounts are estimated based on the average daily `Trader_Activity  * 30 days.`

_Trade statistics object_


```
class trade_statistics_object
{

account_id_type owner;
asset_id_type asset;
timestamp first_trade_date; //first trade date for this asset
share_type total_volume; //total bought asset volume
};
```

### 30-day sliding window average 

In **trade_statistics_object** we need to store the queue of volumes aggregated by 1 day. In other words, for each asset a user should have a queue of 30  daily volumes.

During the first maintenance interval of the day, yesterday's volume will be added to the queue and the  oldest volume will be deleted (for each asset even for assets where there was no trade done).

This way it will cost significant memory usage!!

Technically we can store only the aggregated sum for 30 days and take the first day volumes from the blockchain. This will take less memory but will take more CPU time.


### 30-day volume method

Alternative way would be to store one 30-day volume for each asset for each user.  This way we would only need one 30-day volume  and one aggregated daily volume for today. i.e. only 2 values for each asset.

So the memory requirements will be at least 30 times less than in a previous method.

The algorithm is like this:

```
For example, the 30-day volume is 90. Hence daily average is 3. 
Let's say today's volume is 13. 
First of all, we subtract average daily volume from the 30-day volume.  90-3 =87.
Then we add today's volume.  So the new 30-day volume would be 87+13 = 100.  
A new daily average would be 100/30 = 3.33333
``` 

### 365-day sliding window average 

This is similar to the 30-day sliding window average except the memory requirements for the stored values are SIGNIFICANTLY larger.

## Potential issues
We need to make sure that the suggested calculations will not lead to the significantly wrong results or the blockchain abuse. 
We need to consider border conditions: For example, when there is no statistics at all, unusual statistics of many very small orders, very large order at the beginning of the average window and then no activity registered, etc. 

 
## Technical challenges
Some problems include performance as an additional search and update new index are required.

# Copyright
This document is placed in the public domain.

# See Also
