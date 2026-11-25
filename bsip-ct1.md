    BSIP: TBD
    Title: Mandatory stealth memo on blind outputs
    Authors: xiao93 (https://github.com/xiao93)
    Status: Draft
    Type: Protocol
    Created: 2026-11-25
    Discussion: <to be assigned>
    Replaces: none
    Requires: none

# Abstract

After a hardfork, every new blind output must carry a stealth memo: a `one_time_key` and an `encrypted_memo` of 32 to 1024 bytes. The plaintext recipient field `to` must be absent. Wallets identify a receipt by the private key that decrypts the memo. Historical outputs are unchanged. The hardfork time in this draft is a placeholder, not a mainnet schedule.

# Motivation

A `blind_output` may carry an optional `stealth_confirmation`. The usual wallet flow returns that confirmation out of band and may set `to` to the recipient public key. A scanner that does not already know the recipient cannot find the output, and a set `to` publishes a permanent link from the output to a key.

Wallets that already scan the chain put `one_time_key` and `encrypted_memo` on every blind output and leave `to` unset. This BSIP makes that shape mandatory for outputs created after the hardfork.

Operations affected: `transfer_to_blind` (39), `blind_transfer` (40) until the ring-spend BSIP closes it, and the ring outputs of that BSIP.

# Rationale

The memo format already exists. No new operation tag and no new reflected field are required. Gating the rule on head-block time leaves historical transactions valid.

Forbidding `to` on chain does not stop the sender from remembering who was paid. The sender keeps that link in a local receipt that is not broadcast. A receiver who was not the sender, and any wallet scanning for itself, tries its local private keys against `one_time_key` and accepts the output when the memo checksum matches. That is the same check `stealth_confirmation::memo_data` already defines.

An alternative was to keep `to` optional. That leaves a public link whenever a wallet sets it. Making absence mandatory is the smaller consensus rule.

# Specifications

Let `HARDFORK_BSIP_CT1_TIME` be the placeholder `2030-01-01T00:00:00Z` (`1893456000`). The predicate is `head_block_time >= HARDFORK_BSIP_CT1_TIME`. Replace the constant only in a release that states a real time. Do not ship the placeholder as a mainnet activation.

For every `blind_output` or ring output **created** in a block at or after that time:

1. `stealth_memo` is present.
2. `stealth_memo.to` is absent. A transaction that sets it is invalid.
3. `encrypted_memo` length is in `[32, 1024]` bytes.
4. `one_time_key` is a canonical compressed secp256k1 point: 33 bytes, prefix `0x02` or `0x03`, on the curve, not the point at infinity.

The protocol does not decrypt the memo. The recipient derives the shared secret from `one_time_key` and a local private key, decrypts `encrypted_memo`, and checks `memo_data.check` (the first 4 bytes of the shared-secret hash).

Wallets must identify the receipt by the key that decrypts the memo, not by `to`. After this hardfork `to` is absent on chain, so a lookup of `to` is not available. The sender may store `to` in a local receipt. That local field is not part of the signed operation.

`transfer_from_blind` has no outputs. This BSIP does not add a memo to it.

## Serialization

`stealth_confirmation` is unchanged:

```
public_key_type one_time_key
optional<public_key_type> to
vector<char> encrypted_memo
```

`to` remains an `fc::optional`: a `0` byte when absent, or `1` followed by the key when present. After the hardfork only the `0` form is valid for new outputs. The check runs in the evaluator against head-block time. Unconditional `validate()` of historical operations 39 and 40 does not reject an old `to`.

## Fees

Unchanged for operations 39 and 40: `fee + price_per_output * output_count`. The memo is at most 1024 bytes. Ring operations, specified separately, charge for memo bytes by transaction size.

## Backward compatibility

Blocks before the hardfork validate as they do on 7.0.4-patch3, including outputs with no memo and outputs with `to` set. This BSIP does not change operation tags.

# Discussion

Scanning without a published recipient is the behavior several wallets already implement. Making it consensus removes a class of outputs that those wallets cannot see, and removes the public `to` link.

The cost is that a sender who writes an unreadable memo creates an output the intended recipient will not accept. Consensus does not need the plaintext. The size cap is the denial-of-service bound. A missing memo is a scanning failure, not an inflation bug.

Nodes that never create blind outputs still must upgrade before the hardfork if later BSIPs add operation tags. This BSIP alone does not.

# Summary for Shareholders

New confidential outputs must include an encrypted note and must not publish the recipient's key. Wallets find their own incoming outputs by trying their keys against that note. Old outputs are left as they are. The date in this draft is a placeholder. Nothing here should be activated on mainnet until the related proposals and an external review of the cryptography are accepted.

# Copyright

This document is placed in the public domain.

# See Also

* The ring-spend and Bulletproofs+ drafts filed with this one.
* BSIP 8, Privacy (STEALTH) Mode.
* A Chinese translation of this draft is `bsip-ct1.zh.md`. The English text is normative.
