Benefit Society on a Blockchain
-------------------------------

This improvement would allow individuals to form benefit societies on a blockchain.

A benefit society, fraternal benefit society or fraternal benefit order is a society, 
an organization or a voluntary association formed to provide mutual aid, benefit, 
for instance insurance for relief from sundry difficulties.

The premise of this feature is to be an organizational tool for groups of individuals
working toward a common cause.  The benefit of implementing this on a blockchain is to
enable the formation of of benefit societies that may be difficult or impossible to
operate in the traditional manner.

This feature would not change any existing behavior on the BitShares blockchain and would
only add new operations.

New Operations
--------------

*Create Society* - This operation is similar to create asset. It defines a new society including
a name, description, and any configurable parameters.

*Join Society*  - This operation ties an existing BitShares account to a particular society and 
defines any relevant information associated with society membership. This information may include
the user's real name, age, location, etc.  

*Commit Funds to Society*  - This operation will transfer BTS from an account to a society membership. 
It may be performed by any account. 1% of the BTS transferred will be paid to the BTS reserve pool,
2% of the BTS transfered will be allocated to the referral program, and 2% of the BTS transferred will be
used to buy back a Fee Backed Asset dedicated to this feature.   The society membership will earn Karma within
the society proportional to the BTS contributed.

*Request Payout from Soceity* - This operation will create a payout request. A payout request includes Karma from
the membership account requesting the payout and requires permission of the membership owner. The request includes a
subject, summary, and detailed description explaining the justification.

*Update Payout Request* - This operation can be used to update mutable properties of an existing payout request.

*Approve or Reject Payout Request* - This operation is used to vote for or against paying out a particular request. 
Each member of a society can vote with a weight equal to the amount of karma the account has earned.

*Update Membership Info* - This operation is used to update mutable membership information such as nominating 
a proxy to vote on your behalf.  This is designed to simplify the amount of voting the average member must worry about.

*Fund Payout Request* - This operation will transfer BTS held in the membership account reserve to an approved payout request.


Maitenance Operations
---------------------
The following actions will be performed on a daily basis during a mainteanance interval block.

1. Tally all relevant votes
2. Update management account authority for each society
3. Update management account authority for the fee backed asset 
4. Decay all Karma by the configured percentage

