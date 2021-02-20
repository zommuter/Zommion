# Zommium

Lacking a better idea for a name I'm being narcistic and name this like an element after my nickname for now... Also, let's cut to the chase skipping all introductions for now.

Bitcoin is great yet sucks. Read about it elsewhere in detail if you will, I'll just skim some shortcommings and what I consider solutions:

* Blockchain size: Way too large, growing way too much
* Permanent loss: Lost private keys or intentionally burned BTC are lost forever. Combined with the finite total amount this means BTC will become rarer and rarer once mining is basically over
* PoW: oh dear, poor climate
* Hodling and speculation. It shouldn't be all about money _first_

# Basic ideas

In order to stick to the theme, anything related to Zommium will probably start with Zomm as well, e.g. its blockchain might be named Zommchain etc., and an altcoin might by called Zommcoin, though Zommium as name for both the technology and the basic element should do just fine. Zommium's fundamental ideas are:

* decay
* "bringschuld" for transaction validity
* lots of divide and conquer (d&c) and thus logarithms
* oracling

## Fixing blockchain growth

In order to prevent double spending, the BTC blockchain stores _every single transaction ever made_ so as to provide a gapless proof of ownership. It has grown to several hundred GBs and by design grows every ten minutes by a block size of [currently](https://charts.bitcoin.com/btc/chart/block-size#5moc) more than 1MB (and rising with rising trade volume), which means it'll grow by more than 50GB/year given early 2021's numbers. That's already much to ask for on an average desktop PC, and absolute infeasible on even a modern 2021 smartphone. Of course there are lightweight clients which don't store the entire blockchain but therefore have to trust external services for blockchain integrity, which is suboptimal.

Instead of storing the entire blockchain it is sufficient to keep track of all currently unspent transactions. But given the way transactions are split and merged all the time, that's probably still a big list that will potentially grow forever. There have been several [hundred millions of transactions](https://charts.bitcoin.com/btc/chart/total-transactions#5moc) so far; unfortunately I haven't found a number of unspent transactions. And there's also the matter of burned transactions, be it accidentally or on purpose, plus the unknown amount of those lost due to lost private keys.

By introducing decay, there is no need to store transactions forever, thus blockchain growth is limited. "Bringschuld" means the author of a transaction has to provide part of the data required to verify its validity instead of forcing innocent bystanders to keep track for them. And thanks to d&c anything growing nonetheless at least will only grow logarithmically instead of linearly or even exponentially. Except for the exchange rate of course, hopefully.

## Loss aversion

Decay means anything lost is not lost forever but just for a limited time.

## PoW

One fundamental concept of the bitcoin blockchain is the proof-of-work: A tremendously stupid calculation of hashes in order to proof sufficient investment in the system in order to prevent fraud. Unfortunately this mostly heats up the universe for nothing. In a perfect world the proof-of-work would at least provide useful computations itself, e.g. public CFD simulations or whatnot. Proof-of-stake sounds like a much more viable and realistic alternative though. And of course, instead of reading up on that I just invented my own scheme, which may or may not be a good idea: Oracling

## Hodling/Speculation

"Hodling", i.e. speculative holding on to bitcoins, and speculation in general are not the true purpose of a cryptocurrency. Of course it is impossible to prevent, but the very least thing speculants should do is support the system by participating in Oracling. Yet another reason for the decay.

# Details
