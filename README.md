# Futarchy with Bonding Curve Tokens

This post proposes a self-contained futarchy mechanism for bonding curve tokens.

#### Problem

[Decentralized exchanges](https://ethresear.ch/t/batch-auctions-with-uniform-clearing-price-on-plasma/2554) are likely the best price-finding solution for futarchy markets but, until they've achieved sufficient usability and liquidity, futarchies must be self contained providing their own price-finding mechanism. The current best know solution is use an [LMSR](http://mason.gmu.edu/~rhanson/mktscore.pdf) [automated market maker](https://blog.gnosis.pm/radical-markets-for-elephants-a742916812db) but this presents significant challenge. Each LMSR market must be funded up front in order to provide liquidity. This places a significant burden on the party that needs to provide the funding as well as additional complexity in determining the necessary amount of funding.

#### Bonding Curve Futarchy

A [bonding curve token](https://medium.com/@justingoro/token-bonding-curves-explained-7a9332198e0e) has a built in price-finding mechanism as well as a reserve pool of funds for liquidity. This can be used to create a relatively simple and self-contained futarchy mechanism. The mechanism works like this:

1. Start with a bonding curve token `ABC` where its bonding curve uses `ETH` as the reserve token.
2. A new decision is proposed with a `YES`/`NO` result.
3. Two tokenized events are started allowing the conversion of `ABC` into the outcome tokens `YES-ABC` and `NO-ABC` and `ETH` into `YES-ETH` and `NO-ETH`. If the decision is `YES`, `YES-ABC` tokens can be exchanged for `ABC` tokens and `YES-ETH` can be exchanged for `ETH`. Likewise, if the decision is `NO`, `NO-ABC` tokens can be exchanged for `ABC` tokens and `NO-ETH` can be exchanged for `ETH`.
4. The main bonding curve is halted and two new bonding curves are created that mint YES-ABC and NO-ABC in exchange for their respective reserve tokens YES-ETH and NO-ETH.
5. The main bonding curve's reserve (ETH) is split into YES-ETH and NO-ETH and is used as the reserve for the YES-ABC and NO-ABC bonding curves respectively.
6. Participants trade on the YES-ABC and NO-ABC curves.
7. The decision is resolved using a normal futarchy decision function such as highest price over the last 24 hours.
8. The winning bonding curve's reserve pool is converted back into ETH through the tokenized event and is used as the reserve for the main ABC bonding curve once again. The winning outcome tokens can be exchanged for ABC and the main ABC bonding curve can resume trading as normal.

![alt text](https://raw.githubusercontent.com/levelkdev/bonding-curve-futarchy/master/Bonding%20Curve%20Futarchy.png "Bonding Curve Futarchy")

#### Drawbacks

While this construction doesn't require additional funding for the price-finding mechanism like constructions that rely on LMSR do, it comes with its own drawbacks:

1. It doesn't work for normal tokens that don't have a bonding curve.
2. Allowing the main bonding curve to function during a decision is an unsolved problem and may not be possible.
3. Bonding curves (as well as LMSR markets) are susceptible to front running.
4. New decisions may create a race to be the first to buy into the new bonding curve.
