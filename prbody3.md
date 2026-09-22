**Play it: <https://bank-of-friends-nu.vercel.app>** — no wallet, no signature, no install. You land inside the hall with a Friend already on the marble.

**The First Bank of Friends** — Hunt · [@huntclubhero](https://github.com/huntclubhero) · `huntclubhero.eth`
**Category:** Economy Potential (also relevant: Character Spotlight)
**Source:** <https://github.com/Halldon-Inc/bank-of-friends> · **Research:** <https://bank-of-friends-nu.vercel.app/docs>

Walk any Rare Friend — **Genesis included** — into a banking hall built on pooled NFT-wallet rewards, and pull the lever at the desk to watch a real market-making strategy decide, week after week, that it should not trade.

---

### Why this is not a FriendSDK game

We built one first. Then we measured the wallet.

`readGenerationEligibility` reads `ownerOf` **and** `generation` from the **Generations** contract and requires `generation >= 1`. A Genesis is a different contract and reports 0, so **no FriendSDK game can ever admit a Genesis.**

For this project that is fatal rather than annoying:

| | idle rewards |
| --- | --- |
| **Genesis #259** | **~4,500 RF** |
| six Gen-3s, combined | ~31 RF |

The Genesis *is* the bank. A bank that excludes 99% of its own deposits is not a bank.

Your rules make the SDK optional for *"a launchpad, tool or agent"*, and a market maker is a tool. So FriendSDK is used as a **library** under its Apache-2.0 licence — `renderWorld` draws the hall, `createWorldMovement` handles walking, collision and pathing. What we supply is the identity gate and the character.

**The unlock:** `renderWorld` takes live actors as `rows` of arbitrary bitmap, not a token id. So the character is rasterised from whatever artwork a Friend actually has, which works for *both* collections where the SDK's sprite reader cannot. A Genesis walks the marble. `contracts/test` proves the same in Solidity: a Genesis enrols and is collected from exactly like a Generations Friend, bounded by the same member cap.

### Why the desk refuses

Before writing a market maker we measured whether one could work. All reproducible: `npm run verify` (37 assertions against live chain state), `npm run backtest` (all 8,777 swaps in the pool's history).

**The pool pays its liquidity providers nothing.** `slot0.lpFee = 0` while the hook takes 5% of every swap and routes it to `ActivationManager`. The people who supply the liquidity and the people who collect the fees are different people.

**So nobody supplies it.** Third-party liquidity is **exactly zero** — your seed position is 100.00% of it, in a market doing ~$37.5k/day. One address ever tried: `0x58daec31…` opened a position, closed it **48 seconds later**, tried again, closed that in 46 seconds, and left.

**Every strategy we tested lost money.** Passive LP −43% to −55%. Grid bots −39% to −87%. Buying the dip −63% to −84%. Momentum "won" only by selling RF and sitting in WETH, and still lost 18% to just holding WETH. Genesis NFT making shows a real 21% bid-ask but the floor fell 45% in five days. The Reserve→OpenSea arb **does not exist**: the Reserve has no sell path.

Cause is mechanical. **5% in plus 5% out is a ~10% round trip**, so a completed trade needs a >10% swing *that comes back*. RF didn't swing, it slid 89%.

So the desk is **flat by default**. On the real tape it takes **zero fills** and ends **+0.00% vs hold**. Arm rate across regimes: **21%** overall, 77% in live chop, **0%** in dead calm, a slow bleed or a hard dump.

### The number that reframes it

A grid is two-sided, and **both** sides must clear the minimum economic fill. One Friend's rewards are 94% WETH / 6% RF, putting the RF side at **$4.94** and its slice at **$0.74**, far under the **$8.71** floor. **A single Friend can buy and can never economically sell.** Minimum viable balanced book: **$116**.

That is not a hole in the argument, it *is* the argument as a number: one Friend cannot make a market, pooled Friends can.

### Parameters are derived, not invented

An earlier draft carried numbers I chose and then tuned against synthetic data my own code generated — circular. `npm run derive` now labels every input **MEASURED / DERIVED / CHOICE** with the algebra, grounded in Ho-Stoll (1981), Avellaneda-Stoikov (2008), Grossman-Miller (1988). It caught a **minimum fill size 26× too low**. Two numbers that should have been stated from the start: **break-even grid step is 10.80%**, and a round trip at a 15% step nets **3.79%, not 15%** — the fee takes 75% of the gross move.

### Two things in FriendSDK worth your attention

1. **Genesis holders cannot play any SDK game.** Excluded twice over: wrong contract, and generation 0. That locks out the protocol's most valuable holders.
2. **The Friend picker shows no artwork** — it renders the token label as text, so you choose blind between Friends that look nothing alike. The SDK already has a sprite reader. Ours shows every Friend's on-chain art, which costs nothing: it is already served as a data URI.

### Checks

| check | result |
| --- | --- |
| `npm run verify` | **37/37** against live chain state |
| `forge test` | **20/20**, including two proving a Genesis enrols |
| `npm run backtest:gated` | 0 fills on the real tape; arms on a ranging one |
| `npm run check:lever` | 21% arm rate; 0% in dead or falling markets |
| viewport sweeps | 320px → 2560px, no overflow or clipped text |

### Honest limitations

5.6 days of one token in one downtrend is a small, unusual sample and nothing here is a forecast. The desk **has never traded**; contracts are written, 20/20 tested and **not deployed**. Deposits from anyone but the builder are closed until an external audit — deliberately. The economy in the hall is simulated and includes a **losing outcome**, because a desk that cannot lose is a desk that is lying to you.
