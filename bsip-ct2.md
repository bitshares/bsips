    BSIP: TBD
    Title: Aggregated Bulletproofs+ blind proofs
    Authors: xiao93 (https://github.com/xiao93)
    Status: Draft
    Type: Protocol
    Created: 2026-11-25
    Discussion: <to be assigned>
    Replaces: none
    Requires: TBD (mandatory stealth memo)

# Abstract

After a hardfork that follows the mandatory-memo hardfork, new blind outputs use one aggregated Bulletproofs+ range proof instead of a Borromean proof per output. The proof covers every output of the operation, at a 64-bit range, and is written once. A transaction whose operations are all confidential may be up to 16384 bytes and still no larger than the block cap. Ordinary transactions stay at 2048 bytes. The hardfork time is a placeholder.

# Motivation

Borromean range proofs are large, and a multi-output blind operation carries one of them on each output. Bulletproofs+ (eprint 2020/735) proves many 64-bit values in one proof of a few hundred bytes. One proof per operation is easier to verify and easier to bound.

Range proofs on confidential outputs must be fully verified. How that requirement relates to official releases is the subject of a responsible disclosure to the bitshares-core maintainers and is not described in this BSIP.

This BSIP assumes full verification of the current Borromean proofs, as implemented by 7.0.4-patch3 (`verify_blind_output_range_proofs` after soft-fork time `1795165200`), and replaces Borromean for **new** outputs. The commitment equation is unchanged: `C = blind * G + value * H`, with the same generators as `secp256k1_pedersen_commit`. Balance remains `fc::ecc::verify_sum`. The range proof shows that each new output value is an integer in `[0, 2^64)` consistent with that commitment.

# Rationale

Aggregating per operation, rather than per output, matches the way a blind operation already lists its outputs in one vector. Putting the proof on the first output, and requiring the other `range_proof` fields to be empty, avoids a new field on `blind_output` and keeps historical reflection valid.

The 16-output cap is the maximum aggregation this construction defines (`M <= 16`). A larger payment is split across operations.

The ordinary transaction size limit is 2048 bytes. A 16-output Bulletproofs+ deposit does not fit in 2048 bytes once memos are attached. Raising the global limit would also raise it for unrelated operations. A separate allowance, used only when every operation in the transaction is confidential, lets the deposit through without relaxing the cap for a mixed transaction. The allowance remains bounded by `maximum_block_size`.

Fee parameters of operations 39 and 40 are not extended. Appending a field would change the fee schedule already stored on chain. The existing per-output fee still grows with the output count, and that count is the bound on proof work.

An external cryptographic review of the curve port is required before a real hardfork time is chosen. The port follows Monero 0.18.3.4 `bulletproofs_plus.cc` on secp256k1 with SHA-256. It is not the Monero binary.

# Specifications

Placeholder time: `HARDFORK_BSIP_CT2_TIME` = `2030-01-01T01:00:00Z` (`1893459600`). It is strictly after the mandatory-memo hardfork.

After the hardfork, for `transfer_to_blind` and for `blind_transfer` (until the ring-spend hardfork disables the latter):

1. `1 <= outputs.size() <= 16`.
2. Outputs are sorted by commitment, as today.
3. `outputs[0].range_proof` is one Bulletproofs+ proof, serialized below, of every output commitment in vector order.
4. `outputs[i].range_proof` for `i > 0` is empty.
5. Verification fails closed on an empty proof, a truncated proof, trailing bytes, a non-canonical scalar, a point not on the curve, the wrong number of rounds, or a proof that does not match the commitments.

Before the hardfork, Borromean verification from 7.0.4-patch3 remains in force. A Bulletproofs+ byte string is not a Borromean proof, so it is rejected before this hardfork.

Ring operations carry the same proof in a dedicated `range_proof` field instead of inside the first output.

## Transaction size

`maximum_transaction_size` stays 2048 for ordinary transactions. After this hardfork, a transaction whose operations are all confidential (`transfer_to_blind`, `blind_transfer`, `transfer_from_blind`, and the two ring operations) may be up to `GRAPHENE_CONFIDENTIAL_TX_SIZE_ALLOWANCE` (16384) bytes, and still no larger than `maximum_block_size`. A mixed transaction does not get the allowance. Sixteen blinded outputs with short memos fit. Sixteen outputs with a 1024-byte memo each do not.

## Proof bytes

Scalars are 32-byte big-endian, strictly less than the secp256k1 group order. Points are 33-byte compressed SEC1 (`0x02` or `0x03` || X). Re-encoding must match.

```
offset 0   ASCII "BP+"
offset 3   version = 0x01
offset 4   A      (33)
offset 37  A1     (33)
offset 70  B      (33)
offset 103 r1     (32)
offset 135 s1     (32)
offset 167 d1     (32)
offset 199 rounds (1)    must equal 6 + log2(M)
offset 200 L[0] .. L[rounds-1]   (33 each)
then       R[0] .. R[rounds-1]   (33 each)
```

`M` is the smallest power of two greater than or equal to the number of commitments, and `M <= 16`. No bytes follow `R`. A one-commitment proof has 6 rounds and is 596 bytes. A four-commitment proof (`M = 4`) is 728 bytes.

The proof does not repeat the commitments. The verifier inserts the operation's commitments into the Fiat-Shamir transcript.

## Cryptographic statement

The algorithm is Bulletproofs+ (eprint 2020/735) as structured in Monero 0.18.3.4 `bulletproofs_plus.cc`, on secp256k1.

- `G` is the SEC 2 generator. `H` is secp256k1-zkp generator G2 from `group_impl.h` at commit `bd067945`.
- Generators `Gi[i]` and `Hi[i]` are try-and-increment hash-to-curve of `BTS-BP+-Gi-v1 || uint32le(i)` and `BTS-BP+-Hi-v1 || uint32le(i)`.
- The transcript is SHA-256. The initial domain string is `BTS-BP+-TRANSCRIPT-v1`. Each update hashes `0x01 || scalar` or `0x02 || point`.
- `z` is SHA-256 of the 32-byte scalar `y` alone.
- There is no cofactor clearing. The secp256k1 cofactor is 1.
- Consensus verification uses batch weight 1. Other non-zero weights are not a consensus path.

## Fees

Operations 39 and 40 keep `fee + price_per_output * n`. Default `price_per_output` is `5 * GRAPHENE_BLOCKCHAIN_PRECISION` per output. On one measured Release build, verifying one 64-bit proof took about 5.6 ms (596 bytes) and a 4-output aggregate about 20 ms (728 bytes). The per-output fee is the charge that scales with that work.

## Backward compatibility

`blind_output` reflection is unchanged. Only the bytes inside `range_proof` change, and only after the hardfork. Historical Borromean outputs stay spendable until the ring-spend hardfork. A node that has not upgraded will not accept a Bulletproofs+ proof, so this is a hardfork.

# Discussion

Shareholders are being asked to accept a new proof system for new confidential outputs, plus a narrow size allowance so a full deposit fits in a block. The allowance does not apply to ordinary transfers.

The proof system is a port, not a drop-in of an audited binary. Activation without an external review of that port is out of scope for this draft. The placeholder time is one hour after the memo hardfork so a memo failure can be seen before proof rules change.

Negative tests that must keep passing: an empty proof, an 8-byte truncation, a flipped proof byte, and a proof checked against a different commitment. Those are checks of this proof format. They are not a description of any historical defect.

# Summary for Shareholders

New confidential payments would use a shorter range proof, one per payment instead of one per output. A confidential payment can be larger than today's 2048-byte transaction limit, up to 16 KiB, and still inside the block limit. Ordinary payments stay at 2048 bytes. The date is a placeholder. The new proof code needs an outside cryptographic review before any real date is set.

# Copyright

This document is placed in the public domain.

# See Also

* Mandatory stealth memo draft (`bsip-ct1.md`), which this BSIP requires.
* Ring-spend draft (`bsip-ct3.md`), which reuses this proof.
* Chung, Han, Kim, Seo, eprint 2020/735.
* Responsible disclosure of range-proof verification on official releases: not included here.
* A Chinese translation is `bsip-ct2.zh.md`. The English text is normative.

# Test vectors

Deterministic commitments. The blind is a 32-byte big-endian scalar. The value is a `uint64`.

| blind | value | compressed commitment |
| --- | --- | --- |
| 3 | 1 | `0263ed256bcac0f48fabdd28184aaf0dc05c5d0019bf88ab78f842c437b5159830` |
| 9 | 2 | `03c80827b14ea6b15ba26b5ff36c46013201582440417539a1914abe20fb574de7` |

One proof of those two openings. It begins with ASCII `BP+` (`42502b01`). Other proofs of the same openings are also valid. The prover's randomness is not consensus. Flipping any one byte must fail verification.

```
42502b010283f1d32d6a7f437a9ce14dbce8581d5f406f38288843254bac61d961f9596d8f03496c0b47779691820ebd5e3d66894edc2870642fe3321d0899fe69cea141d26902523df5578a87db1e154fc959ff68f493e1915ddc6c394cb6039d1810d2623f0c0f393f64d67a057c711657e53fcd6ea9a1895efb764afdb4e632535bd8097eb60243ead7a4db8d56bf80cd0644772119452d8c8c095883cc818f1bb1e05cd5ce2e68313de1d60d11043ffc656f73e4281d900ad77dd2d86a15086904db7a35190703edd0e834e1116ff0a71749d4473ac8cd4928ed64e6851a58798676d739f2fc6a03c9036ac68f3a0d149ce4ddfd338ef9dfd2daef179162c98ec8eddb5ae898d8d7032553c64facdb9a022b3e3a14ad2e85c75c7c385380945a61ada0a4fcc8c7ac48030d4db44a67bc83798c89b9ef41705b963d0a618d7361c29dbe2257e8dccf274c03e31ad43a51301f6a27a8b989afa9eb043a1cce554896bbc3478b972364498da803be605ca541e388f46ed38ade78bbf407b4ff5b8f3f16bfd4b8e1826dfbbb789702e31a8c0c6f1a27b4753afa7ac43407ea33cbc27b2e9b04cbc932905c80fc2c450218b4a11433112ea94e58937d1286f80522a4f02bbf1f02d52f387f00e7300621028c5dd04f4e4e2022f5e74187313fb6f9e6216b7233a3524d9409da2f28dadb3802a1af41dc3b60d6670171658d3007dc855495035bb036f865e78bcc396fb7dcbf03941fd7719f7509a2fbfd1c5eec20ea3102fa44365c04e1156212e5a820ee8783038f3ecdaff3e3d33459cb31d653a8bb698e90d17c1efd364bec3b68c569f3f496021a3fea21247d2b56c56b66004901573b9f4fd21385a1916101dcae1ba5ef211403e84e220dcffe96e3d52d4dd93561148851712307e63c64e9a896f46763aff928
```
