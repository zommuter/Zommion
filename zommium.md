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
* "bringschuld" for transaction validity (maybe, maybe not)
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

## Decay

What kind of decay makes sense? Linear? Exponential? That's the fundamental question to settle. But ultimately exponential decay just boils down to a linearly decaying exponent, so the technical implementation is not that different. In the end, decay associates a countdown to each transaction, but how shall that countdown relate to the tokens? Does this even need to be set in stone now? If transactions just track their expiration date, they cannot be merged or split meaningfully but only their ownership be transferred. Yet somehow that could mean the countdown effectively _is_ the value, so we'd obtain a linear decay. However, merged or split transactions should not have a different lifetime, which would happen if the countdowns of merged/split transactions were simply to be added/subtracted (e.g. two transactions with a countdown value of 4 each should both vanish after 4 cycles, but merging them to a countdown value of 8 would change that, whereas keeping the total countdown at 4 would evaporate half the value by merging). Hence the decay must be exponential, as much as it pains the user - it doesn't have to be fast though. In order to prevent rounding errors the linear countdown is still kept, but the transaction additionally needs to store its base value. And since rounding errors must not occur when merging or splitting transactions, they also store a base time, in the simplest case dating back to the oldest merged-in transaction. Let's do some math:

Each _pure_ transaction stores its initial value $v_0$, and an offset cycle number $c_0$. The half-life $\lambda$ may actually also be stored per-transaction, though it might be kept globally fixed in order to prevent a confusing zoo of "particles" (to be discussed later). As long as the global cycle count $c$ is smaller than an expiration count $c_e$ (and the transaction has not been invalidated otherwise by being spent), the current transaction value is $v(c) = v_0\cdot 2^{-(c-c_0)/\lambda}$. $c_0$ should be chosen such that $v_0$ is the smallest possible integer (remember $v_0 2^{-c_0/\lambda}$ is a single constant; how it's split up between $v_0$ and $c_0$ is a free choice). One thing to be determined next is whether the sum of pure transactions remains pure or not, i.e. whether $\sum_j v_j 2^{-c_j/\lambda}$ can be expressed via integer valued $v_\text{tot},c_\text{tot}$ as well. Another question is whether $c_e$ must be part of the transaction (and how is it determined for merges then?), or is it redundantly encodeable via a hard cutoff at e.g. $v(c)<1$? One idea here might be instead defining an offset value $v_-$ to be subtracted from $v(c)$ above, and once that value becomes negative the transaction has expired, or just store a cutoff value per transaction. The hard-coded cutoff of one would be similar to $v_-=1$, but it seems pretty obvious that for merges the offsets or cutoffs must be summed up as well.

Where does the decayed value go? Into the Oracling pot of course!

## Bringschuld

"Bringschuld" is a German expression for "debt to deliver". Instead of having _everyone_ store all still valid transactions, it's up to the one performing a new transaction to provide all data necessary to prove the validity. To be discussed is whether that might not cause unnecessarily large blocks, however. If the table of valid transactions is properly stored in a sensible d&c way though, that might be enough.

## D&C

Anything not required in detail anymore, such as ancient blocks containing only expired transactions, can be discarded. But even before that, the blocks shall be collected in partial hash trees and only their tips shall be stored. That way even the Bringschuld will only cause logarithmically growing data for ancient proofs.

## Oracling

Mining is such a horrible waste of computational power. The proposal of oracling is simple: Each transaction to the oracle pot (including decay and transaction fees or only if done on purpose?) deterministically gets a hash assigned. After a certain amount of cycles (to be determined such that abusing the algorithm as perverted proof-of-work becomes infeasible) the pot is split among oracle transactions according to their proximity to the current blockchain hash and the value invested. Details to be done...
