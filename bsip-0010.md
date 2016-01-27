    BSIP: 0010
    Title: Percentage-based transfer fee solution based on CER
    Authors: Jakub Zarembinski <jakub.zarembinski@neura.sx>
    Status: Final
    Type: Protocol
    Created: 2015-12-27
    Discussion: https://bitsharestalk.org/index.php/topic,20789.0.html
    Worker: t.b.d.
    
# Summary for Shareholders
This worker proposal aims to introduce a percentage-based transfer fee solution as an alternative to the current flat rate. The main goal is to make BitShares viable for transferring smaller amounts - the current flat transfer rate restrains this, as for smaller transfers the flat fee makes up a significant part of the amount being sent. As we have it know, most of the blockchain's blocks are empty, which from a business perspective is a clear waste of company's resources.

The aim of this proposal is to make use of the fact that users generally are more willing to pay higher transfer fees when they send bigger amounts (and this is the situation when we can make profit) whereas they will not accept relatively big transfer fees for smaller amounts (and this is the situation when we need to forgo profit). An overall increase in the network's revenue and liquidity is expected, as lower fees should open up BitShares to micro-transactions.  

This proposal is also important for another reason: it paves the way for adding new features to Graphene without the direct involvement of its core developers. The documentation produced as a result of this work will help other developers to join the ecosystem and offer proposals for further features.

# Abstract
Any percentage-based transfer fee scheme needs some kind of basis for deriving the actual BTS value of the amount being transferred. The core idea of this approach is to derive this value from the *Core Exchange Rate* (CER) which is defined by the asset's issuer.  A complementary modification in the referral system is also needed in order to prevent abuse on the part of referrers.

If the minimum transfer fee is kept at a reasonable level (i.e. around 6 BTS), the only actors actually affected by this proposal are referral businesses, as their revenue stream will be negatively affected for smaller transfers but in exchange positively affected for larger ones. The effect for issuers will only be positive or none at all, as for them this an opt-in feature. As for shareholders in general, the change will be positive because BitShares will become more competitive in terms of transfer fees when compared to other systems. 

If we keep the minimum fee at around 6 BTS, the financial effect for the blockchain itself will be neutral, as all profits (or losses) will be absorbed by the referrers. However, an overall increase in the network's revenue is expected, as it is safe to assume that, once small transfers get significantly cheaper, there will be more of them and each of them will contribute to the network's revenue.

# Motivation
Having a flat transfer fee of 30 BTS for non-LTM users, currently BitShares cannot be regarded as a competitive payment solution for transfers below the equivalent of $5. The current fee in this case is above 2%, which is way more than any legacy system wants to charge its customers.

There are two important objectives this proposal aims to achieve:
* to allow users to pay lower transfer fees for smaller transfers (while charging them more on bigger transfers)

* to allow referrers to earn extra income associated with bigger transfers (while taking away from them the current income associated with smaller transfers).

Apart from that, we would like to make detailed documentation a crucial part of this proposal, thus offering an important benefit for the whole ecosystem: a step-by-step description how to implement new features without relying on CNX so that other coders & entrepreneurs can follow our example. The documentation will entail:
* programming context: a brief overview of Graphene's architecture
* what changes have been made in the code and their rationale
* how unit testing and testnet verification has been approached
* how the whole implementation has been deployed

This documentation will hopefully become the foundation of a larger tutorial for Graphene developers. The work on this material has already been started and a brief preview can be accessed [here](https://neura-sx.gitbooks.io/graphene-book/content/).

# Rationale
The simplest way to solve the "high transfer fee" problem would be to lower transfer fees for all assets. However this would effectively negatively affect the referral program. On the other hand, a simple percentage-based transfer fee system is not doable, as for most assets we do not have a viable way to determine their exact value at any given time. This is mainly due to the fact that most markets are very illiquid but also due to the difficulty of establishing an asset's price in a manner which is deterministic enough to reliably prevent unintended hard-forks.

Also, a percentage-based transfer fee system, if introduced without any modifications to the referral program, would open up the possibility for referrers to game the system by transferring small amounts back and forth between their own accounts just to collect the referral award at the expense of issuers. Therefore, for assets enjoying a percentage-based transfer fee system, referrers' revenue structure has to be percentage-based as well.

# Specifications
#### The assumptions
The following description uses a fictional asset called BAX but this solution can be applied to all assets (i.e. both public and private SmartCoins, UIAs and FBAs) and all non-stealth transfers. Also, what is described below refers to pay-as-you-go users. For LTM users exactly the same rules apply, except for the fact that in this case the referrer's income is redirected to the user's cash-back fund.

> Please note that values used in the description below are just an initial proposal, the actual values will be determined be the committee.

#### The concept
The transfer fee can be paid either in BTS or BAX.

1. If the user chooses to pay the fee in BTS:
  * The transfer fee is equal to 1% of the BTS value of the BAX amount being transferred, adjusted by a floor (i.e. minimum fee) and a cap (i.e. maximum fee): 6 BTS and 300 BTS respectively. So effectively the transfer fee is 1% but it is never less than 6 BTS and never more than 300 BTS.
  * To establish the BTS value of the transfer, CER is used.
  * The fee paid by the user is split as follows: 6 BTS goes to the network, any excess above 6 BTS goes to the referrer.
  * The issuer does not earn anything.
2. If the user chooses to pay the fee in BAX:
Let *F* be the transfer fee (expressed in BTS) which was calculated as described in (1).
  * To establish the BAX equivalent of *F*, CER is used.
  * The issuer receives the entire fee paid by the user in BAX.
  * The following amounts are deducted from the asset fee pool: 6 BTS goes to the network, and (*F* - 6) BTS goes to the referrer.
  * The issuer makes a profit or loss, depending on CER being above or below the actual BTS/BAX exchange rate.

#### The main issue
What happens when the issuer attempts to artificially lower transfer fees on his asset by pretending his asset is not worth much and deliberately keeping CER below the actual BTS/BAX exchange rate?
- for the user: it's cheaper for him to pay the fee in BTS rather than in BAX
- for the issuer: he makes a profit on all those transfers for which the user, for whatever reasons, decides to pay the more expensive fee in BAX (instead of the cheaper one in BTS)
- for the referrer: no matter if the user pays the fee in BAX or BTS, he is deprived of a large part of the referral reward, as due to distorted CER, transfers are interpreted by the system as involving smaller value than they actually are.

#### How is this issue solved?
By keeping CER artificially low, the issuer will effectively discourage users from using his own asset because transfer fees will only be low when paid in BTS, so the process of transferring the asset will be dependent on users having BTS in their wallets to cover the fees. Furthermore, if the issuer distorts CER too much, he'll not be able to benefit from it himself as users will start avoiding paying the fees in BAX. Also, it will be bad PR for the issuer to value his own asset below the market rate. At least in case of SmartCoins (both public & private) issuers are not likely to be wiling to sabotage their own assets in this way.

#### Additional abuse prevention mechanism
However, if it turns out that a large number of issuers try to game the system, in the future we can introduce the following mechanism aimed at preventing abuse on the part of issuers:
* To enable percentage-based transfer fees, the issuer will be obliged to keep an open ask order on the BTS:BAX market and thus allow anybody to buy BAX at e.g. 120% of CER. As a result, if CER is out of sync with the market rate by more than 20%, the issuer will incur a significant loss.
* If the issuer is not willing to comply with the above requirement, the default flat transfer fee structure will be applied.

> Please note that this additional abuse prevention mechanism will not be implemented with this worker proposal. If there is a need for it in the future, we will consider it as a separate worker proposal.

#### This is an opt-in feature
As the issuer is the only entity that actually controls CER, the percentage-based transfer fee is meant to be an opt-in feature decided by the issuer. It is safe to assume that most issuers will find it beneficial for their assets because:
* assets which are worth something will benefit from having a reasonable pricing structure for transfers
* assets which are worth nothing or almost nothing (i.e. most UIAs) will be able to be transferred almost for free (for 6 BTS instead of the current 30 BTS).
However, if an issuer for some reasons does not find the above features beneficial, he will be able to keep the existing flat fee structure.

#### The intended workflow

Action | Result
--- | ---
An issuer sets his asset to the percentage-based transfer fee mode. | After the change, fees for all transfers on that asset will be calculated with the new algorithm.
An issuer sets his asset to the flat transfer fee mode. | After the change, fees for all transfers on that asset will be flat.
The Committee adjusts the fee parameters. | After the change, for transfers on all assets which have applied percentage-based transfer fee mode, fees will be calculated with the new parameters.
The Committee sets the percentage based fee mode for a SmartCoin. | After the change, fees for all transfers on that asset will be calculated with the new algorithm.
The Committee sets the flat fee mode for a SmartCoin. | After the change, fees for all transfers on that asset will be flat.
A user makes a transfer on an asset assigned to the flat fee mode.  | The user will be charged flat fee, 20% of the fee goes to the network, 80% of the fee goes to the referral program and is split among the parties inside the referral program (registrar, referrer etc).
A user makes a transfer on an asset assigned to the percentage based fee mode. | The user will be charged a percentage fee, the minimum fee goes to network, the rest (if there is anything left) goes to the referral program and is split among the parties inside the referral program.

# Discussion
Being voluntary for issuers, the above proposal is actually targeted to the referral businesses: do they perceive it as a beneficial change for the ecosystem and a fair deal for them? If we stick to the parameter values used above, they will need to forgo the referral income on transfers below the equivalent of $2 but will substantially increase their income on transfers above the equivalent of $10. In the range between $2 and $10 they will get on average half of the income they have now. Nevertheless, the main benefit will be indirect: it's much easier to sell a reasonably priced product.

As for the BitShares network itself, the overall impact of this proposal will be positive, as the network's resources will be better utilized. With the current flat fees, most of the blocks are empty. Percentage-based fees will not fix this waste immediately, but will be a step in the right direction and a real opportunity for businesses based on small tips and micro-payments.

# Areas covered
This proposal covers the entire implementation of percentage-based fees.  
In particular, the following aspects will be covered:
* coding
* code review by CNX
* unit testing and testnet testing
* deployment
* full documentation & guidelines for similar projects
* GUI support

# Known issues and limitations
There are the following issues and limitations that cannot be overcome due to technical reasons:
* The percentage-based fee scheme cannot be applied to stealth transfers (as the amount being transferred is unknown, thus it's impossible to calculate its BTS value).

* The percentage-based fee scheme cannot be applied to the core currency i.e. BTS. However all SmartCoins (both public and private), UIAs and possibly also FBAs (once they get implemented) will be covered.

* There might be a negative UX in a very rare situation: when CER is being updated by the issuer between the moment the user hits `Transfer` and the moment s/he hits `Confirm`. In this situation the user might receive an error message about not supplying sufficient funds to cover the transfer fee and will have to redo the transfer.

* ~~In case of committee-controlled SmartCoins, occasionally there might be a small discrepancy between the current value of CER and the value of CER enclosed in the committee proposal to switch a given SmartCoin to the percentage-based mode.~~ EDIT: We will probably be able to offer a patch to the code-base, which makes it possible to update an asset without changing CER, and thus fix this issue.

*  If the committee decides to apply percentage-based fees to SmartCoins, our partners (e.g. exchanges, gateways etc) need to notified in advance, so that they have time to adjust their applications, and make sure those applications and their pricing policy are compatible with percentage-based fees.

# Copyright
This document is placed in the public domain.
