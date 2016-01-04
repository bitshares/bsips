The purpose of this BSIP is to minimize the cost incurred by the network doing unnecessary operations every maitenance interval.

Every hour votes are tallied and 99% of the time there is no meaningful change. Under this proposal, votes would only
be tallied when requested by a user and at most once per maitenance interval. The fee for talleying votes would be around $5.

Users of the system can determine whether or not there is a $5 difference in votes that would justify a retally. A worker, witness,
or committee member who got voted in would pay $5 to have the vote counted.

The side effect of this behavior would be to greatly improve reindexing performance, the vast majority of which is spent tallying
votes that never change.

Witnesses producing blocks on the maitenance interval can pre-tally the votes and compare them to the last value. They can then
indicate whether or not anything changed significantly in the block they produce.  

If there is no one who stands to benefit by more than $5 then chances are it is an unnecessary update to the voting totals. 
