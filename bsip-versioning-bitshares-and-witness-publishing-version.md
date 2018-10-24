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
signatures and timestamping transactions by including them in blocks. Per
block one elected witness is signing the block and applying it to the database,
hence changing the state of the database. 

In certain situations the state can only be changed correctly when the
signing witness is running the newest version of the BitShares code. Currently
there is no way to tell whether a witness-node has an outdated version running
or not. 

To solve this problem this BSIP introduces a way to show the version of the witness
on the blockchain. Furthermore a versioning scheme, which is currently
not part of the BitShares code, will be introduced to represent the version.

# Motivation

In case of a protocol upgrade a witness running an old version of the BitShares code
will be unable to valide a transaction, which was introduced with the 
new upgrade. The transaction won't be applied to the blockchain.

The goal is to decrease the chance of an outdated witness-node. This could 
be achieved by monitoring the blockchain and notifying the nodes when a
outdated version is detected.

# Rational

The used versioning scheme is `Major.Minor.Patch`.
- Major: consensus changes
- Minor: changes not impacting consensus
- Patch: hotfix

The `witness_object` is added a `running_version` variable. It shows the current running 
version of the witness node. 

With every signed block it will be checked that, the `running_version`
hasn't changed. If `running_version != current_version`, `running_version` is replaced by 
`current_version`. Also `current_version` is included in to the current block, so everyone can see
that it has upgraded its code.

The binary version is tracked by setting a `GRAPHENE_BLOCKCHAIN_VERSION` variable in the
`config.hpp`. (https://github.com/bitshares/bitshares-core/compare/master...Dimfred:feature/bitshares-versioning#diff-d18d5e1e065d6fef202d7648640ee99dR26)

# Specifications

For the pupose of this BSIP the `version` class was created. It contains the three numbers of the
version and the `hardfork_time`, which shows when the hardfork occured. ( https://github.com/bitshares/bitshares-core/compare/master...Dimfred:feature/bitshares-versioning#diff-61f3e7649478bc43b6164aa7f4f3f617R1 )

For clarification all occurences of `HARDFORK_XXX_TIME` were changed to `HARDFORK_XXX_VERSION`.
To not change the old behaviour in the code, operator overloads for adding/subtracting time offsets for: 
- `uint_32`
- `fc::microseconds`
- `fc::time_point_sec`
were added to the `version` class. 

When the version of the witness changes, it is included in to the block.
https://github.com/bitshares/bitshares-core/compare/master...Dimfred:feature/bitshares-versioning#diff-b74c6390562c34c6cc3ec703e1dce295R431

Also the `witness_object.running_version` is modified in this case.
https://github.com/bitshares/bitshares-core/compare/master...Dimfred:feature/bitshares-versioning#diff-b74c6390562c34c6cc3ec703e1dce295R550

# Discussion

To be found in the forum - see link above.

# Summary for Shareholders

This BSIP introduces a way to publish the running version of a witness node to the blockchain.
It greatly reduces the risk of having an outdated version running on a witness node.

# Copyright

This document is placed in the public domain.

# See Also

