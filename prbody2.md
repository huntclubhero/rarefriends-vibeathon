**Play it: <https://halldon-inc.github.io/bank-of-friends/>** — needs a browser wallet holding a hardwired Generations NFT (gen ≥ 1) on Robinhood mainnet. Everything in it is simulated.

**The First Bank of Friends** — Hunt · [@huntclubhero](https://github.com/huntclubhero) · `huntclubhero.eth`
**Category:** Economy Potential (also relevant: Character Spotlight) · **FriendSDK v0.1.2**
**Source:** <https://github.com/Halldon-Inc/bank-of-friends>

Walk your Rare Friend into a banking hall built on pooled NFT-wallet rewards, and pull the lever at the trading desk to watch a real market-making strategy decide, week after week, that it should not trade.

---

### The hall

A banking hall authored as a **custom world**, because the supplied presets are gardens, rooftops and caverns and none of them is a bank. Four windows:

- **Teller** — deposit a simulated 1 RF slip
- **The vault** — the pooled book, and the arithmetic for why one Friend cannot make a market
- **Trading desk** — pull the lever: a week of market rolls and the strategy decides
- **The ledger** — the findings that produced the gates

**The trading desk is not a mock.** `game/strategy.mjs` is byte-identical to the `lib/strategy.mjs` the backtests and keeper use, and `npm run check:game-sync` fails the build if that ever stops being true. When the desk stands down in the game it stands down for exactly the reason it would with real money, and it names the gate that blocked it.

### Why it refuses

We set out to build a market maker. Before writing it we measured whether one could work. All of this is reproducible: `npm run verify` runs **37 assertions** against live chain state, `npm run backtest` replays all **8,777 swaps** in the pool's history.

**The pool pays its liquidity providers nothing.** `slot0.lpFee = 0`, while `Hook.FEE_BPS() = 500` routes 5% of every swap to `ActivationManager`. The people who supply the liquidity and the people who collect the fees are different people.

**So nobody does.** Third-party liquidity is **exactly zero**; the Market's seed is 100.00% of it, in a market doing ~$37.5k/day. Exactly one address ever tried — `0x58daec31…` opened a concentrated position, closed it **48 seconds later**, tried again, closed that in 46 seconds, and left.

**Every strategy we tested lost money.** Passive LP −43% to −55%. Grid bots −39% to −87%. Buying the dip −63% to −84%. Momentum "won" only by selling RF and sitting in WETH, and still lost 18% to just holding WETH. Genesis NFT making shows a real 21% bid-ask but the floor fell **45% in five days**. And the Reserve→OpenSea arb everyone assumes exists **does not**: the Reserve has no sell path.

Cause is mechanical. **5% in plus 5% out is a ~10% round trip**, so a completed trade needs a >10% swing *that comes back*. RF didn't swing, it slid 89%.

### The parameters are derived, not invented

An earlier draft carried numbers I had chosen and then tuned against synthetic data my own code generated — circular. `npm run derive` now labels every input **MEASURED / DERIVED / CHOICE** and shows the algebra, grounded in Ho-Stoll (1981), Avellaneda-Stoikov (2008) and Grossman-Miller (1988).

It caught three real errors, including a **minimum fill size that was 26× too low** ($0.33 → $8.71: the old figure ignored that a round trip nets 3.79%, not 100%).

Two numbers that should have been stated from the start: the **break-even grid step is 10.80%** (`s > 1/(1-f)² − 1` at f=5%), and a round trip at a 15% step nets **3.79%, not 15%** — the fee takes 75% of the gross move.

### The finding that reframes it

A grid is two-sided, so **both** sides must clear the minimum fill. One Friend's rewards are 94% WETH / 6% RF, which puts the RF side at **$4.94** and its slice at **$0.74**, far under the $8.71 floor. **A single Friend can buy and can never economically sell.** Minimum viable balanced book: **$116**.

That is not a hole in the argument, it *is* the argument, as a number rather than a slogan: one Friend cannot, pooled Friends can, and protocol-wide idle rewards are roughly $30,000.

### One thing that may be useful to your team

There is a Laffer curve on the hook fee. Any venue cheaper than 5% must multiply volume to keep Friend holders whole: **5.0×** at 1%/side, 2.5× at 2%, 1.7× at 3%. We can't prove from 5.6 days where the peak sits, but the break-even is exact, and 5% may be above the revenue-maximising rate.

### Checks

| check | result |
| --- | --- |
| `npx friendsdk check` | **valid**, expected reward 1.0354 RF, max 1.2 RF |
| `npx friendsdk test` | **PASS** at 960px and 360px |
| `npm run verify` | **37/37** against live chain state |
| `forge test` | **18/18** on the contract safety properties |
| `npm run backtest:gated` | 0 fills on the real tape; arms on a ranging one |
| visual sweep 320→2560px | **70/70** against production |

### Honest limitations

5.6 days of one token in one downtrend is a small, unusual sample and nothing here is a forecast. The desk has never traded; contracts are written, 18/18 tested, and **not deployed**. Deposits from anyone but the builder are closed until an external audit — deliberately. At 360px all four station prompts are visible at once and overlap; only the one in reach activates, but it is busy. The lever's regimes are generated, labelled as such, and shaped from the measured sweep.

The economy includes a **losing outcome at 7%**, because a desk that cannot lose is a desk that is lying to you. The 55% stand-down rate matches the sweep.
