    BSIP: xxxx
    Title: Auction feature
    Authors: Valera Cogut <info@valeracogut.com> and contributors
    Status: Draft
    Type: Protocol
    Created: 2018-07-14
    Discussion: 
    Worker: 1.14.x

# Abstract

Wiki <https://en.wikipedia.org/wiki/Auction>

An auction is a process of buying and selling goods or services by offering them up for bid, 
taking bids, and then selling the item to the highest bidder. 
The open ascending price auction is arguably the most common form of auction in use today.
Participants bid openly against one another, with each subsequent bid required to be higher 
than the previous bid.An auctioneer may announce prices, bidders may call out their bids 
themselves (or have a proxy call out a bid on their behalf), or bids may be submitted 
electronically with the highest current bid publicly displayed.In a Dutch auction, 
the auctioneer begins with a high asking price for some quantity of like items; the price 
is lowered until a participant is willing to accept the auctioneer's price for some 
quantity of the goods in the lot or until the seller's reserve price is met.
While auctions are most associated in the public imagination with the sale of antiques, 
paintings, rare collectibles and expensive wines, auctions are also used for commodities, 
livestock, radio spectrum and used cars. In economic theory, an auction may refer to any 
mechanism or set of trading rules for exchange.

# Specifications

This proposal is about new Auction feature which can extend BitShares financial platform in order to make BitShares more
popular and more flexible to cover the best financial possibilities.

## Types

### Primary

There are traditionally four types of auction that are used for the allocation of a single item[citation needed]:

1. English auction, also known as an open ascending price auction. This type of auction is arguably the most common form of auction in use today.[1] Participants bid openly against one another, with each subsequent bid required to be higher than the previous bid.[2] An auctioneer may announce prices, bidders may call out their bids themselves (or have a proxy call out a bid on their behalf), or bids may be submitted electronically with the highest current bid publicly displayed.[2] In some cases a maximum bid might be left with the auctioneer, who may bid on behalf of the bidder according to the bidder's instructions.[2] The auction ends when no participant is willing to bid further, at which point the highest bidder pays their bid.[2] Alternatively, if the seller has set a minimum sale price in advance (the 'reserve' price) and the final bid does not reach that price the item remains unsold.[2] Sometimes the auctioneer sets a minimum amount by which the next bid must exceed the current highest bid.[2] The most significant distinguishing factor of this auction type is that the current highest bid is always available to potential bidders.[2] The English auction is commonly used for selling goods, most prominently antiques and artwork,[2] but also secondhand goods and real estate.
2. Dutch auction also known as an open descending price auction.[1] In the traditional Dutch auction the auctioneer begins with a high asking price for some quantity of like items; the price is lowered until a participant is willing to accept the auctioneer's price for some quantity of the goods in the lot or until the seller's reserve price is met.[2] If the first bidder does not purchase the entire lot, the auctioneer continues lowering the price until all of the items have been bid for or the reserve price is reached. Items are allocated based on bid order; the highest bidder selects their item(s) first followed by the second highest bidder, etc. In a modification, all of the winning participants pay only the last announced price for the items that they bid on.[1] The Dutch auction is named for its best known example, the Dutch tulip auctions. ("Dutch auction" is also sometimes used to describe online auctions where several identical goods are sold simultaneously to an equal number of high bidders.[18]) In addition to cut flower sales in the Netherlands, Dutch auctions have also been used for perishable commodities such as fish and tobacco.[2] The Dutch auction is not widely used, except in market orders in stock or currency exchanges, which are functionally identical.[1]
3. Sealed first-price auction or blind auction,[19] also known as a first-price sealed-bid auction (FPSB). In this type of auction all bidders simultaneously submit sealed bids so that no bidder knows the bid of any other participant. The highest bidder pays the price they submitted.[1][2] This type of auction is distinct from the English auction, in that bidders can only submit one bid each. Furthermore, as bidders cannot see the bids of other participants they cannot adjust their own bids accordingly.[2] From the theoretical perspective, this kind of bid process has been argued to be strategically equivalent to the Dutch auction.[20] However, empirical evidence from laboratory experiments has shown that Dutch auctions with high clock speeds yield lower prices than FPSB auctions.[21][22] What are effectively sealed first-price auctions are commonly called tendering for procurement by companies and organisations, particularly for government contracts and auctions for mining leases.[2]
4. Vickrey auction, also known as a sealed-bid second-price auction.[23] This is identical to the sealed first-price auction except that the winning bidder pays the second-highest bid rather than his or her own.[24] Vickrey auctions are extremely important in auction theory, and commonly used in automated contexts such as real-time bidding for online advertising, but rarely in non-automated contexts.[2]

### Secondary

Most auction theory revolves around these four "standard" auction types. However, many other types of auctions exist, including:

1. Multiunit auctions sell more than one identical item at the same time, rather than having separate auctions for each. This type can be further classified as either a uniform price auction or a discriminatory price auction.
2. All-pay auction is an auction in which all bidders must pay their bids regardless of whether they win. The highest bidder wins the item. All-pay auctions are primarily of academic interest, and may be used to model lobbying or bribery (bids are political contributions) or competitions such as a running race.[25]
3. Auction by the candle. A type of auction, used in England for selling ships, in which the highest bid laid on the table by the time a burning candle goes out wins.
4. Bidding fee auction, also known as a penny auction, often requires that each participant must pay a fixed price to place each bid, typically one penny (hence the name) higher than the current bid. When an auction's time expires, the highest bidder wins the item and must pay a final bid price.[26] Unlike in a conventional auction, the final price is typically much lower than the value of the item, but all bidders (not just the winner) will have paid for each bid placed; the winner will buy the item at a very low price (plus price of rights-to-bid used), all the losers will have paid, and the seller will typically receive significantly more than the value of the item.[27]
5. Buyout auction is an auction with an additional set price (the 'buyout' price) that any bidder can accept at any time during the auction, thereby immediately ending the auction and winning the item.[28] If no bidder chooses to utilize the buyout option before the end of bidding the highest bidder wins and pays their bid.[28] Buyout options can be either temporary or permanent.[28] In a temporary-buyout auction the option to buy out the auction is not available after the first bid is placed.[28] In a permanent-buyout auction the buyout option remains available throughout the entire auction until the close of bidding.[28] The buyout price can either remain the same throughout the entire auction, or vary throughout according to rules or simply as decided by the seller.[28]
6. Combinatorial auction is any auction for the simultaneous sale of more than one item where bidders can place bids on an "all-or-nothing" basis on "packages" rather than just individual items. That is, a bidder can specify that he or she will pay for items A and B, but only if he or she gets both.[29] In combinatorial auctions, determining the winning bidder(s) can be a complex process where even the bidder with the highest individual bid is not guaranteed to win.[29] For example, in an auction with four items (W, X, Y and Z), if Bidder A offers $50 for items W & Y, Bidder B offers $30 for items W & X, Bidder C offers $5 for items X & Z and Bidder D offers $30 for items Y & Z, the winners will be Bidders B & D while Bidder A misses out because the combined bids of Bidders B & D is higher ($60) than for Bidders A and C ($55).
7. Generalized second-price auction and Generalized first-price auction
8. Unique bid auctions
9. Many homogenous item auctions, e.g., spectrum auctions
10. Japanese auction is a variation of the English auction. When the bidding starts no new bidders can join, and each bidder must continue to bid each round or drop out. It has similarities to the ante in Poker.[30]
11. Lloyd's syndicate auction.[31]
12. Mystery auction is a type of auction where bidders bid for boxes or envelopes containing unspecified or underspecified items, usually on the hope that the items will be humorous, interesting, or valuable.[32][33] In the early days of eBay's popularity, sellers began promoting boxes or packages of random and usually low-value items not worth selling by themselves.[34]
13. No-reserve auction (NR), also known as an absolute auction, is an auction in which the item for sale will be sold regardless of price.[35][36] From the seller's perspective, advertising an auction as having no reserve price can be desirable because it potentially attracts a greater number of bidders due to the possibility of a bargain.[35] If more bidders attend the auction, a higher price might ultimately be achieved because of heightened competition from bidders.[36] This contrasts with a reserve auction, where the item for sale may not be sold if the final bid is not high enough to satisfy the seller. In practice, an auction advertised as "absolute" or "no-reserve" may nonetheless still not sell to the highest bidder on the day, for example, if the seller withdraws the item from the auction or extends the auction period indefinitely,[37] although these practices may be restricted by law in some jurisdictions or under the terms of sale available from the auctioneer.
14. Reserve auction is an auction where the item for sale may not be sold if the final bid is not high enough to satisfy the seller; that is, the seller reserves the right to accept or reject the highest bid.[36] In these cases a set 'reserve' price known to the auctioneer, but not necessarily to the bidders, may have been set, below which the item may not be sold.[35] If the seller announces to the bidders the reserve price, it is a public reserve price auction.[38] In contrast, if the seller does not announce the reserve price before the sale but only after the sale, it is a secret reserve price auction.[39] The reserve price may be fixed or discretionary. In the latter case, the decision to accept a bid is deferred to the auctioneer, who may accept a bid that is marginally below it. A reserve auction is safer for the seller than a no-reserve auction as they are not required to accept a low bid, but this could result in a lower final price if less interest is generated in the sale.[36]
15. Reverse auction is a type of auction in which the roles of the buyer and the seller are reversed, with the primary objective to drive purchase prices downward.[40] While ordinary auctions provide suppliers the opportunity to find the best price among interested buyers, reverse auctions give buyers a chance to find the lowest-price supplier. During a reverse auction, suppliers may submit multiple offers, usually as a response to competing suppliers’ offers, bidding down the price of a good or service to the lowest price they are willing to receive. By revealing the competing bids in real time to every participating supplier, reverse auctions promote “information transparency”. This, coupled with the dynamic bidding process, improves the chances of reaching the fair market value of the item.[41]
16. Senior auction is a variation on the all-pay auction, and has a defined loser in addition to the winner. The top two bidders must pay their full final bid amounts, and only the highest wins the auction. The intent is to make the high bidders bid above their upper limits. In the final rounds of bidding, when the current losing party has hit their maximum bid, they are encouraged to bid over their maximum (seen as a small loss) to avoid losing their maximum bid with no return (a very large loss).
17. Silent auction is a variant of the English auction in which bids are written on a sheet of paper. At the predetermined end of the auction, the highest listed bidder wins the item.[42] This auction is often used in charity events, with many items auctioned simultaneously and "closed" at a common finish time.[42][43] The auction is "silent" in that there is no auctioneer selling individual items,[42] the bidders writing their bids on a bidding sheet often left on a table near the item.[44] At charity auctions, bid sheets usually have a fixed starting amount, predetermined bid increments, and a "guaranteed bid" amount which works the same as a "buy now" amount. Other variations of this type of auction may include sealed bids.[42] The highest bidder pays the price he or she submitted.[42]
18. Top-up auction is a variation on the all-pay auction, primarily used for charity events. Losing bidders must pay the difference between their bid and the next lowest bid. The winning bidder pays the amount bid for the item, without top-up.
19. Walrasian auction or Walrasian tâtonnement is an auction in which the auctioneer takes bids from both buyers and sellers in a market of multiple goods.[45] The auctioneer progressively either raises or drops the current proposed price depending on the bids of both buyers and sellers, the auction concluding when supply and demand exactly balance.[46] As a high price tends to dampen demand while a low price tends to increase demand, in theory there is a particular price somewhere in the middle where supply and demand will match.[45]
20. Amsterdam auctions, a type of premium auction which begins as an English auction. Once only two bidders remain, each submits a sealed bid. The higher bidder wins, paying either the first or second price. Both finalists receive a premium: a proportion of the excess of the second price over the third price (at which English auction ended).[47]
21. Other auctions: Other auction types also exist, such as Simultaneous Ascending Auction[48] Anglo-Dutch auction,[49] Private value auction,[50] Common value auction

### Time requirements

Each type of auction has its specific qualities such as pricing accuracy and time required for preparing and 
conducting the auction. The number of simultaneous bidders is of critical importance. 
Open bidding during an extended period of time with many bidders will result in a final bid 
that is very close to the true market value. Where there are few bidders and each bidder is allowed only one bid, 
time is saved, but the winning bid may not reflect the true market value with any degree of accuracy. 
Of special interest and importance during the actual auction is the time elapsed from the moment that 
the first bid is revealed to the moment that the final (winning) bid has become a binding agreement.

### Characteristics

Auctions can differ in the number of participants:

1. In a supply (or reverse) auction, m sellers offer a good that a buyer requests
2. In a demand auction, n buyers bid for a good being sold
3. In a double auction n buyers bid to buy goods from m sellers

Prices are bid by buyers and asked (or offered) by sellers. Auctions may also differ by the procedure for bidding (or asking, as the case may be):

1. In an open auction participants may repeatedly bid and are aware of each other's previous bids.
2. In a closed auction buyers and/or sellers submit sealed bids

Auctions may differ as to the price at which the item is sold, whether the first (best) price, the second price, the first unique price or some other. Auctions may set a reservation price which is the least/maximum acceptable price for which a good may be sold/bought.
Without modification, auction generally refers to an open, demand auction, with or without a reservation price (or reserve), with the item sold to the highest bidder.

# Discussion

## Bidding strategy

An 18th century Chinese meiping porcelain vase. Porcelain has long been a staple at art sales. In 2005, a 14th-century Chinese porcelain piece was sold by the Christie's for £16 million, or US$28 million. It set a world auction record for any ceramic work of art.[60]
Katehakis and Puranam provided the first model[61] for the problem of optimal bidding for a firm that in each period procures items to meet a random demand by participating in a finite sequence of auctions. In this model an item valuation derives from the sale of the acquired items via their demand distribution, sale price, acquisition cost, salvage value and lost sales. They established monotonicity properties for the value function and the optimal dynamic bid policy. They also provided a model[62] for the case in which the buyer must acquire a fixed number of items either at a fixed buy-it-now price in the open market or by participating in a sequence of auctions. The objective of the buyer is to minimize his expected total cost for acquiring the fixed number of items.

### Bid shading

Bid shading is placing a bid which is below the bidder's actual value for the item. Such a strategy risks losing the auction, but has the possibility of winning at a low price. Bid shading can also be a strategy to avoid the Winner's curse.

### Chandelier or rafter bidding
This is the practice, especially by high-end art auctioneers,[63] of raising false bids at crucial times in the bidding in order to create the appearance of greater demand or to extend bidding momentum for a work on offer. To call out these nonexistent bids auctioneers might fix their gaze at a point in the auction room that is difficult for the audience to pin down.[64] The practice is frowned upon in the industry.[64] In the United States, chandelier bidding is not illegal. In fact, an auctioneer may bid up the price of an item to the reserve price, which is an unstated amount the consignor will not sell the item for. However, the auction house is required to disclose this information.

In the United Kingdom this practice is legal on property auctions up to but not including the reserve price, and is also known as off-the-wall bidding.[65]

### Collusion

This section does not cite any sources. Please help improve this section by adding citations to reliable sources. Unsourced material may be challenged and removed. (March 2009) (Learn how and when to remove this template message)
Whenever bidders at an auction are aware of the identity of the other bidders there is a risk that they will form a "ring" or "pool" and thus manipulate the auction result, a practice known as collusion. By agreeing to bid only against outsiders, never against members of the "ring", competition becomes weaker, which may dramatically affect the final price level. After the end of the official auction an unofficial auction may take place among the "ring" members. The difference in price between the two auctions could then be split among the members. This form of a ring was used as a central plot device in the opening episode of the 1979 British television series The House of Caradus, 'For Love or Money', uncovered by Helena Caradus on her return from Paris.

A ring can also be used to increase the price of an auction lot, in which the owner of the object being auctioned may increase competition by taking part in the bidding him or herself, but drop out of the bidding just before the final bid. In Britain and many other countries, rings and other forms of bidding on one's own object are illegal. This form of a ring was used as a central plot device in an episode of the British television series Lovejoy (series 4, episode 3), in which the price of a watercolour by the (fictional) Jessie Webb is inflated so that others by the same artist could be sold for more than their purchase price.

In an English auction, a dummy bid is a bid made by a dummy bidder acting in collusion with the auctioneer or vendor, designed to deceive genuine bidders into paying more. In a first-price auction, a dummy bid is an unfavourable bid designed so as not to become the winning bid. (The bidder does not want to win this auction, but he or she wants to make sure to be invited to the next auction).

In Australia, a dummy bid (shill, schill) is a criminal offence, but a vendor bid or a co-owner bid below the reserve price is permitted, if clearly declared as such by the auctioneer. These are all official legal terms in Australia, but may have other meanings elsewhere. A co-owner is one of two or several owners (who disagree among themselves).

In Sweden and many other countries there are no legal restrictions, but it will severely hurt the reputation of an auction house that knowingly permits any other bids except genuine bids. If the reserve is not reached this should be clearly declared.

In South Africa auctioneers can use their staff or any bidder to raise the price as long as its disclosed before the auction sale. The Auction Alliance[66] controversy focused on vendor bidding and it was proven to be legal and acceptable in terms of the South African consumer laws.

### Suggested opening bid (SOB)

This section does not cite any sources. Please help improve this section by adding citations to reliable sources. Unsourced material may be challenged and removed. (May 2014) (Learn how and when to remove this template message)
There will usually be an estimate of what price the lot will fetch. In an ascending open auction it is considered important to get at least a 50-percent increase in the bids from start to finish. To accomplish this, the auctioneer must start the auction by announcing a suggested opening bid (SOB) that is low enough to be immediately accepted by one of the bidders. Once there is an opening bid, there will quickly be several other, higher bids submitted. Experienced auctioneers will often select an SOB that is about 45 percent of the (lowest) estimate. Thus there is a certain margin of safety to ensure that there will indeed be a lively auction with many bids submitted. Several observations indicate that the lower the SOB, the higher the final winning bid. This is due to the increase in the number of bidders attracted by the low SOB.

A chi-squared distribution shows many low bids but few high bids. Bids "show up together"; without several low bids there will not be any high bids.

Another approach to choosing an SOB: The auctioneer may achieve good success by asking the expected final sales price for the item, as this method suggests to the potential buyers the item's particular value. For instance, say an auctioneer is about to sell a $1,000 car at a sale. Instead of asking $100, hoping to entice wide interest (for who wouldn't want a $1,000 car for $100?), the auctioneer may suggest an opening bid of $1,000; although the first bidder may begin bidding at a mere $100, the final bid may more likely approach $1,000.

# History

The word "auction" is derived from the Latin augeō, which means "I increase" or "I augment".
For most of history, auctions have been a relatively uncommon way to negotiate the exchange of goods and commodities. 
In practice, both haggling and sale by set-price have been significantly more common.
Indeed, before the seventeenth century the few auctions that were held were sporadic.
Nonetheless, auctions have a long history, having been recorded as early as 500 B.C.
According to Herodotus, in Babylon auctions of women for marriage were held annually.
The auctions began with the woman the auctioneer considered to be the most beautiful and progressed to the least. 
It was considered illegal to allow a daughter to be sold outside of the auction method.
During the Roman Empire, following military victory, Roman soldiers would often drive a spear 
into the ground around which the spoils of war were left, to be auctioned off. 
Later slaves, often captured as the "spoils of war", were auctioned in the forum under the sign of the spear, 
with the proceeds of sale going towards the war effort.
The Romans also used auctions to liquidate the assets of debtors whose property had been confiscated.
For example, Marcus Aurelius sold household furniture to pay off debts, the sales lasting for months.
One of the most significant historical auctions occurred in the year 193 A.D. when the entire Roman Empire 
was put on the auction block by the Praetorian Guard. On 28 March 193, the Praetorian Guard first killed 
emperor Pertinax, then offered the empire to the highest bidder. Didius Julianus outbid everyone else for 
the price of 6,250 drachmas per guard,[citation needed] an act that initiated a brief civil war. 
Didius was then beheaded two months later when Septimius Severus conquered Rome.
From the end of the Roman Empire to the eighteenth century auctions lost favor in Europe,
while they had never been widespread in Asia.

# Copyright

This document is placed in the public domain.
