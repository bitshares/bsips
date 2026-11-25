    BSIP: TBD
    Title: CLSAG ring spends for blind balances
    Authors: xiao93 (https://github.com/xiao93)
    Status: Draft
    Type: Protocol
    Created: 2026-11-25
    Discussion: <to be assigned>
    Replaces: none
    Requires: TBD (mandatory stealth memo), TBD (Bulletproofs+)

# Abstract

Two new operations spend confidential balances inside a fixed ring of 16 commitments of the same asset. Each input carries a CLSAG signature and a key image. Spent commitments stay on chain and remain available as decoys. Pseudo-output commitments balance the values. After the hardfork, `blind_transfer` and `transfer_from_blind` can no longer spend. The hardfork time is a placeholder, two hours after the mandatory-memo time and one hour after the Bulletproofs+ time.

# Motivation

A blind spend today names the commitment and deletes it. An observer learns which output was consumed. Amounts are hidden. The link is not.

A ring signature hides which member was spent. The key image `I = p * Hp(P)` is determined by the spend key, so a second spend of that key is rejected without naming the member. Commitments are not deleted. Deleting one would shrink the decoy set and would also reveal which member was chosen. Keeping them is how the anonymity set can grow.

# Rationale

The construction follows CLSAG (eprint 2019/654) as structured in Monero 0.18.3.4 `verRctCLSAGSimple`, on secp256k1 with SHA-256 and the existing Pedersen generators. It is not the Monero binary. An external review of the port is required before a real hardfork time is chosen.

The ring size is a consensus constant, 16, not a chain parameter. A parameter would let a one-member ring be valid, which hides nothing. The cost of a fixed size is liveness: an asset with fewer than 16 commitments cannot be ring-spent yet. Deposits still work.

Legacy spends must stop at this hardfork. A ring spend does not delete the commitment. If operation 40 or 41 could still spend it, the same coin would pay twice. That closure is part of the proposal.

Outputs whose owner is not a single key of weight 1 cannot be ring members. They have to be spent with the legacy operations before this hardfork, or they become unspendable. New outputs created for rings use a single key.

Tags 78 and 79 are appended. Tags 0 through 77 do not move.

# Specifications

Placeholder time: `HARDFORK_BSIP_CT3_TIME` = `2030-01-01T02:00:00Z` (`1893463200`). Both new operations are invalid before that time, including inside a proposal. Their fee parameters cannot be set before that time.

## Constants

| Name | Value |
| --- | --- |
| Ring size | 16 |
| Maximum inputs | 8 |
| Maximum outputs | 16 |
| Memo rules | the mandatory-memo BSIP |
| Range proof | the Bulletproofs+ BSIP |

## Common input rules

Each ring input contains:

- `members`: exactly 16 commitments, strictly increasing. Duplicates are rejected.
- Each member exists as a blinded balance, has the same asset id as `fee.asset_id`, and its owner is a single key of weight 1. The spend key is read from that owner. It is not taken from the transaction.
- `pseudo_commitment`: a Pedersen commitment to the same value as the real member, under a blinding factor the signer knows.
- `key_image`: the compressed point `I`, equal to the image inside the CLSAG.
- `clsag`: the signature bytes below.

A key image already stored in the key-image index (object space `2.19.x`) rejects the transaction. Two inputs in one operation cannot share an image.

The signed message is SHA-256 of:

```
ASCII "BTS-RING-MSG-v1"
|| pack(fee) || pack(public_amount) || pack(to) || pack(public_blind)
|| for each input: pack(members) || pack(pseudo_commitment) || pack(key_image)
|| pack(output commitments) || pack(range_proof)
```

For the blind-to-blind operation, `public_amount`, `to`, and `public_blind` are zero. The signature scalars are not in the message. The key image does not depend on the message, so the signer computes `I` first and then the message.

CLSAG statement, for a secret index `l` that is not transmitted:

- `P[l] = p * G`, where `P[i]` is the spend key of member `i`.
- `C[i] = member_commitment[i] - pseudo_commitment`, and `C[l] = z * G`.
- `I = p * Hp(P[l])` and `D = z * Hp(P[l])`.
- `Hp` is try-and-increment hash-to-curve of `BTS-CLSAG-Hp-v1` followed by the compressed point, tagged `0x02`.

Aggregation hashes use `BTS-CLSAG-AGG-P-v1` and `BTS-CLSAG-AGG-C-v1` over the spend keys, the member commitments (not the differences `C[i]`), `I`, `D`, and the pseudo-output. The round hash uses `BTS-CLSAG-ROUND-v1`, those two rings, the pseudo-output, `0x01 || message`, then `L` and `R`.

## ring_blind_transfer (tag 78)

```
asset fee
vector<ring_input> inputs
vector<ring_output> outputs
vector<char> range_proof
```

A ring output is a commitment, a single-key owner, and a stealth memo as specified by the memo BSIP. After that hardfork, `to` is absent on chain. Wallets match the output to a local key by decrypting the memo.

Balance: `sum(pseudo_commitment) - sum(output commitment) - fee * H = 0`.

`range_proof` is one Bulletproofs+ over the outputs in order. Outputs are strictly sorted by commitment. `1 <= outputs.size() <= 16`.

Apply: credit the fee through the temp-account fee path; insert each key image; do not remove input commitments; create a blinded balance per output; decrease `confidential_supply` by the fee.

## ring_transfer_from_blind (tag 79)

```
asset fee
asset amount
account_id_type to
blind_factor_type blinding_factor
vector<ring_input> inputs
vector<ring_output> changes
vector<char> range_proof
```

`amount.asset_id` equals `fee.asset_id`, and `amount` is positive. Let `public = fee + amount`. The commitment `blind(blinding_factor, public)` is appended for the balance equation only. It is a public value and is not range-proved. Change outputs, if any, are range-proved. If there is no change, `range_proof` is empty.

Apply: credit `amount` to `to`; insert key images; do not remove input commitments; create blinded balances for the changes; decrease `confidential_supply` by `amount + fee`.

## CLSAG bytes

```
ASCII "CL"
version = 0x01
I (33) D (33) c1 (32) n (1 byte) s[0] .. s[n-1] (32 each)
```

`n` is 16. Scalars are canonical. `I` and `D` are not the point at infinity. `c1` is not zero. The length is exact.

## Fees

```
required = fee
         + price_per_input * input_count
         + floor(packed_bytes * price_per_kbyte / 1024)
```

Defaults when the schedule has no entry yet: `fee = 10 * precision`, `price_per_kbyte = 1 * precision`, `price_per_input = 20 * precision`. The data-fee product must not exceed `GRAPHENE_MAX_SHARE_SUPPLY`. At core precision 100000, one input is 10 BTS plus 20 BTS plus 1 BTS per KiB. Input count is at most 8. A pure confidential transaction may use the 16384-byte allowance from the Bulletproofs+ BSIP, and must still fit in the block.

## Decoy selection (wallets)

A listing of blinded balances by asset walks commitments that were not deleted, including spent ones. A wallet must:

- select members of one asset only;
- include its real output;
- sort the 16 commitments before signing;
- draw from the whole pool, not only from its own outputs and not only from the newest commitments;
- refuse to sign until that asset has at least 16 commitments;
- remember key images it has published so it does not retry them.

## Migration

An existing blinded balance can be a ring member when the owner is a single key and the wallet knows the spend scalar, the amount, and the blind, from the memo or from a backup.

At the hardfork, `blind_transfer` and `transfer_from_blind` can no longer spend. `transfer_to_blind` can still create outputs, under the memo and Bulletproofs+ rules. Outputs that are not a single key must be spent before the hardfork.

## Backward compatibility

Old operation tags and layouts are unchanged. The blinded-balance object is unchanged; an index by `(asset_id, commitment)` is derived. The key-image object is new and empty on old databases. Pre-hardfork blocks contain neither new operation. An old node cannot deserialize tags 78 and 79. This is a hardfork.

# Discussion

The privacy gain is a ring of 16 within one asset, not a single anonymity set across all assets. Amounts stay hidden by the commitment and the range proof. The graph of "which of these 16" is what the ring hides, and only if wallets follow the decoy rules above.

The costs are the hardfork, the stuck-funds case for non-single-key owners, and the wait until an asset has 16 commitments. Those are the reasons the legacy spend path stays open until the hardfork and closes at the hardfork rather than earlier.

Shareholders should not treat a local test suite as a substitute for an external review of the CLSAG and Bulletproofs+ port.

# Summary for Shareholders

Confidential coins could be spent without pointing at the exact coin, by hiding it among 16 coins of the same asset. Spending the same coin twice would still fail. Old confidential outputs that are not controlled by a single key need to be moved out before this change, or they cannot be spent afterward. The date is a placeholder. The new cryptography needs an outside review first.

# Copyright

This document is placed in the public domain.

# See Also

* Mandatory stealth memo draft (`bsip-ct1.md`).
* Bulletproofs+ draft (`bsip-ct2.md`).
* Goodell, Noether, et al., eprint 2019/654.
* BSIP 8, and the earlier stealth-phase drafts BSIP 50 through 54, which this draft does not replace.
* A Chinese translation is `bsip-ct3.zh.md`. The English text is normative.

# Test vectors

Primitive tests:

- An honest CLSAG of ring size 4 and of ring size 16 verifies.
- The same spend key yields the same key image in two signatures.
- A different message, a flipped response scalar, a pseudo-output of a different amount, and a substituted commitment fail.
- A truncated encoding and an empty encoding fail parsing.

Chain tests:

- Sixteen outputs deposited under one Bulletproofs+ proof, within the 16384-byte allowance and above 2048 bytes.
- One ring spend leaves the spent commitment in the index, inserts the key image, and reduces `confidential_supply` by the fee only.
- A second operation carrying that key image fails.
- Replaying the block by popping it and pushing it restores the key image.
- `transfer_from_blind` after the hardfork fails.
- The required fee increases when a second input is added.
- A data fee whose product exceeds `GRAPHENE_MAX_SHARE_SUPPLY` fails.
- A ring whose size is not 16 fails in validation.
- `ring_transfer_from_blind` whose fee asset and amount asset differ fails in validation.
- A signed ring that includes a commitment of another asset fails.
- Two copies of the same member, and two inputs that reuse one key image, fail.
