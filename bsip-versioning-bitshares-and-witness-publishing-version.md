    BSIP:       <NUMBER>
    Title:      Create versioning for the Bitshares Blockchain
    Authors:    Fabian Schuh <https://github.com/xeroc>
    Status:     Draft
    Type:       Protocol
    Created:    2018-10-11
    Discussion: https://github.com/bitshares/bitshares-core/issues/1173
    Worker:     <Id of worker proposal>

# Abstract

In BitShares witnesses serve as a neutral third party. They are validating
signatures and timestamping transactions by including them in blocks. One
elected witness is signing the block and applying it to the database, hence
changing the state of the database.

Due to consensus requirements, all witnesses need to agree on the set of rules,
i.e., the protocol.  Currently there is no way to tell whether a witness-node
has an outdated version running or not.

To solve this problem this BSIP introduces a way to store the version of the witness
on the blockchain. Furthermore a versioning scheme, which is currently
not part of the BitShares code, will be introduced to represent the version.

# Motivation

In case of a protocol upgrade a witness running an old version of the BitShares
code will be unable to validate a transaction, which was introduced with the
new upgrade. The transaction won't be applied to the blockchain. If the majority
of the block producers agreed on a version different from the one used locally, the
local blockchain will get stuck with no meaningful reason provided.

The goal is to provide means to the node operators to identify that the
blockchain protocol has advanced. This could be achieved by monitoring the
blockchain and notifying the nodes when a outdated version is detected.

# Specifications

## Witness Object

The `witness_object` is added a `running_version` variable. It shows the
current running version of a particular witness.

With every signed block it will be checked that, the `running_version` hasn't
changed. If `running_version != current_version`, `running_version` is replaced
by `current_version`. Also `current_version` is included in to the current
block, so everyone can see that it has upgraded its code.

## Versioning

The used versioning scheme is `Major.Minor.Patch`.
- Major: consensus changes
- Minor: changes not impacting consensus
- Patch: hotfix

For the pupose of this BSIP a `version` class is used. It contains the three
numbers of the version and the `hardfork_time`, which shows when the hardfork
occured.

For clarification all occurences of `HARDFORK_XXX_TIME` are changed to
`HARDFORK_XXX_VERSION`.  To not change the old behaviour in the code, operator
overloads for adding/subtracting time offsets for: 
- `uint_32`
- `fc::microseconds`
- `fc::time_point_sec`
were added to the `version` class. 

When the version of the witness changes, it is included in to the block.
Also the `witness_object.running_version` is modified in this case.

# Discussion

To be found in the forum - see link above.

# Summary for Shareholders

This BSIP introduces a way to publish the running version of a witness to the
blockchain. It greatly reduces the risk of having an outdated version running
on a witness node.

# Copyright

This document is placed in the public domain.
