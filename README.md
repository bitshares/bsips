# BitShares Improvement Proposal

BSIP stands for BitShares Improvement Proposal. A BSIP is a design document
providing information to the BitShares community, or describing a new feature for
BitShares or its processes or environment. BSIPs provide a concise
technical specification of features and a rationale for them.

The distinct document [BSIP Purpose and Guidelines](bsip-0001.md) gives a more
detailed explanation.

# Available BSIPs

Number             | Title                                                    | Owner             | Type           | Status
-------------------|----------------------------------------------------------|-------------------|----------------|--------
[1](bsip-0001.md)  | BSIP Purpose and Guidelines                              | Fabian Schuh      | Informational  | Draft
[2](bsip-0002.md)  | Refund Create Order Fees on Cancel                       | Daniel Larimer    | Protocol       | Superseded by [26](bsip-0026.md)
[3](bsip-0003.md)  | Maker / Taker Market Fees Flag                           | Daniel Larimer    | Protocol       | Obsoleted by [81](bsip-0081.md)
[4](bsip-0004.md)  | Distribute Market Fees on Core Asset to Referral Program | Daniel Larimer    | Protocol       | Obsoleted by [43](bsip-0043.md)
[5](bsip-0005.md)  | Expiring Votes for Witnesses                             | Daniel Larimer    | Protocol       | 	Superseded by [22](bsip-0022.md)
[6](bsip-0006.md)  | Market Maker Incentivization                             | Daniel Larimer    | Protocol       | Draft
[7](bsip-0007.md)  | Fee Backed Assets (FBA)                                  | Daniel Larimer    | Protocol       | Installed
[8](bsip-0008.md)  | Privacy (STEALTH) Mode                                   | Daniel Larimer    | Protocol       | Installed
[9](bsip-0009.md)  | Benefit Society                                          | Fabian Schuh      | Protocol       | Draft
[10](bsip-0010.md) | Percentage-based transfer fee solution based on CER      | Jakub Zarembinski | Protocol       | Accepted
[15](bsip-0015.md) | Disable negative voting on workers                       | Daniel Larimer    | Protocol       | Installed
[16](bsip-0016.md) | Optimization to Force Settlement Parameters of BitCNY    | Jerry Liu         | Protocol       | Installed
[17](bsip-0017.md) | Revive BitAsset after Global Settlement                  | Peter Conrad      | Protocol       | Draft
[18](bsip-0018.md) | Revive BitAsset through buying Settlement Pool           | Fabian Schuh      | Protocol       | Installed
[19](bsip-0019.md) | Introducing profit sharing/dividends to Bitshares (MPA only)        | Customminer       | Protocol       | Deferred
[20](bsip-0020.md) | Introducing profit sharing/dividends to Bitshares (UIA only)        | Customminer       | Protocol       | Deferred
[21](bsip-0021.md) | Introducing the 'Coin-Age' statistic to Bitshares assets | Customminer       | Protocol       | Draft
[22](bsip-0022.md) | Vote Decay for Witnesses, Committee Members & Proxies    | Customminer       | Protocol       | Draft
[23](bsip-0023.md) | Sharedropping an UIA against an external cryptocurrency distribution snapshot        | Customminer       | Protocol       | Draft
[24](bsip-0024.md) | Locking Bitshares away as 'Bitshares Influence' for voting privileges on the BTS DEX       | Customminer       | Protocol       | Draft
[25](bsip-0025.md) | Transaction Flat-Rates with Weighted Rate-Limitation     | Fabian Schuh | Protocol | Draft
[26](bsip-0026.md) | Refund Order Creation Fee in Originally Paid Asset on Cancel | Abit More | Protocol | Installed
[27](bsip-0027.md) | Asset Issuer Reclaim Fee Pool Funds                          | Abit More | Protocol | Installed
[28](bsip-0028.md) | Worker Proposal Improvements                                 | Bill Butler | Protocol | Draft
[29](bsip-0029.md) | Asset issuer change to require owner authority               | Fabian Schuh | Protocol | Installed
[30](bsip-0030.md) | Always Allow Increasing Collateral Ratio If Debt Not Increased | Abit More | Protocol | Installed
[31](bsip-0031.md) | Update Short Position's Margin Call Price After Partially Called Or Settled | Abit More | Protocol | Installed
[32](bsip-0032.md) | Always Match Orders At Maker Price                                          | Abit More | Protocol | Installed
[33](bsip-0033.md) | Maker Orders With Better Prices Take Precedence                             | Abit More | Protocol | Installed
[34](bsip-0034.md) | Always Trigger Margin Call When Call Price Above Or At Price Feed           | Abit More | Protocol | Installed
[35](bsip-0035.md) | Mitigate Rounding Issue On Order Matching                                   | Abit More | Protocol | Installed
[36](bsip-0036.md) | Remove expired price feeds on maintenance interval                          | oxarbitrage | Protocol | Installed
[37](bsip-0037.md) | Allow new asset name to end with a number                                   | oxarbitrage | Protocol | Installed
[38](bsip-0038.md) | Add target collateral ratio option to short positions                       | Abit More | Protocol | Installed
[39](bsip-0039.md) | Automatically approve proposals by the proposer                             | Fabian Schuh | Protocol | Draft
[40](bsip-0040.md) | Custom active permission                             | Stefan Schießl | Protocol | Accepted - Milestone 1
[41](bsip-0041.md) | Reduce MSSR of bitCNY from 1.1 to 1.05               | Jerry Liu      | Informational | Superseded by [59](bsip-0059.md)
[42](bsip-0042.md) | Adjust price feed to influence trading price of SmartCoins                  | Abit More | Protocol | Rejected
[43](bsip-0043.md) | Market Fee Sharing                                                          | OpenLedgerApp | Protocol | Installed
[44](bsip-0044.md) | Hashed Time-Locked Contract                                                 | Ryan R. Fox | Protocol | Installed
[45](bsip-0045.md) | Add bitAsset Backing Collateral flag/permission                             | Customminer | Protocol | Draft
[46](https://github.com/bitshares/bsips/pull/111) | Escrow Concepts                              | Taconator | Informational | Accepted
[47](bsip-0047.md) | Vote Proxies for Different Referendum Categories and explicit voting operation                 | Fabian Schuh | Protocol | Draft
[48](https://github.com/bitshares/bsips/pull/115) | Add Flag to Asset to Prevent Manipulating Max Supply | Fabian Schuh | Protocol | Draft
[50](https://github.com/bitshares/bsips/issues/88) | Stealth development, Phase II                              | Chris Sanborn | Informational | Draft
[51](https://github.com/bitshares/bsips/issues/89) | New operations for Confidential Asset (CA) transactions    | Chris Sanborn | Protocol      | Draft
[52](https://github.com/bitshares/bsips/issues/90) | Ring signatures for untraceability of Stealth transactions | Chris Sanborn | Protocol      | Draft
[53](https://github.com/bitshares/bsips/pull/116) | Blockchain scanning for inbound Stealth transactions    | Chris Sanborn | Informational (Client Protocol) | Draft
[54](https://github.com/bitshares/bsips/issues/92) | Deterministic addresses for Stealth wallets                | Chris Sanborn | Informational | Draft
[55](https://github.com/bitshares/bsips/issues/93) | Metadata hiding via Garlic Routing and other means         | Chris Sanborn | Informational | Draft
[57](bsip-0057.md) | Managed Vesting Balances | Blockchain Projects BV | Protocol | Draft
[58](bsip-0058.md) | Global Settlement Protection Through Price Feeding | Jerry Liu | Informational | Accepted
[59](bsip-0059.md) | Adjustment of MSSR and MCR Through Voting | Jerry Liu | Informational | Accepted
[60](bsip-0060.md) | BitShares URI scheme | John Titor, Stefan Schießl, Abit More | Informational (Client Protocol) | Accepted
[61](bsip-0061.md) | Operation to Update Limit Orders | Nathan Hourt | Protocol | Draft
[62](bsip-0062.md) | Close Short Position | Stefan Schießl | Protocol | Approved
[63](bsip-0063.md) | Short-lived Unidirectional Payment Channels | Christopher J. Sanborn | Informational | Draft
[64](bsip-0064.md) | Optional HTLC preimage length, HASH160 addition, and memo field | John Jones, Abit More | Protocol | Installed
[65](https://github.com/bitshares/bsips/pull/149) | Fix Locked Accounts | OpenLedger | Protocol | Draft
[66](https://github.com/bitshares/bsips/pull/132) | Sharedrop Operation | OpenLedger | Protocol | Draft
[67](https://github.com/bitshares/bsips/pull/133) | Dynamic Market Fees | OpenLedger | Protocol | Draft
[68](https://github.com/bitshares/bsips/pull/134) | Market Fee Based Asset | OpenLedger | Protocol | Draft
[69](bsip-0069.md) | Additional Assert Predicates | Christopher J. Sanborn | Protocol | Draft
[70](bsip-0070.md) | Peer-to-Peer Leveraged Trading | George Harrap, Michel Santos, Peter Conrad | Protocol | Draft
[71](bsip-0071.md) | Add "Prevent Global Settlement" Flag for Smartcoin  | Jerry Liu | Protocol | Draft
[72](bsip-0072.md) | Tanks and Taps: A General Solution for Smart Contract Asset Handling | Nathan Hourt | Protocol | Draft
[73](bsip-0073.md) | Match Force-Settlement Orders with Margin Calls and Limit Orders | Abit More | Protocol | Draft
[74](bsip-0074.md) | Margin Call Fee Ratio | Jerry Liu | Protocol | Installed
[75](bsip-0075.md) | Asset Owner Defines MCR and MSSR Values | John Jones | Protocol | Installed
[76](bsip-0076.md) | Committee-Defined SmartAsset Collateral Threshold | Abit More | Informational | Draft
[77](bsip-0077.md) | Require Higher CR When Creating / Adjusting Debt Positions | Abit More | Protocol | Installed
[81](bsip-0081.md) | Simple Maker-Taker Market Fees | Abit More | Protocol | Installed
[84](bsip-0084.md) | Elections Based on non-Core Asset | Peter Conrad | Protocol | Draft
[85](bsip-0085.md) | Maker Order Creation Fee Discount | Abit More | Protocol | Installed
[86](bsip-0086.md) | Share market fees to the network | Abit More | Protocol | Installed
[87](bsip-0087.md) | Force Settlement Fee | Jerry Liu | Protocol | Installed


