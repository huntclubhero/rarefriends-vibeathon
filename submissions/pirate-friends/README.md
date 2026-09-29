![Pirate Friends battle](https://raw.githubusercontent.com/Halldon-Inc/pirate-friends/main/docs/media/battle.png)

# Pirate Friends

**Fire your Generations. Sink their ship. Keep their hold.**

**Play:** https://halldon-inc.github.io/pirate-friends/ (needs a wallet on Robinhood mainnet holding a hardwired
Generations NFT, generation 1 or higher; the FriendSDK runtime checks this before play)

**Project**
Pirate Friends, a cannon battle game built on FriendSDK v0.1.3.

**Builder / contact**
Hunt &middot; GitHub [@huntclubhero](https://github.com/huntclubhero) &middot; wallet `huntclubhero.eth`

**Category**
Token Activity

**One sentence**
Your Rare Friend captains a pirate ship and fires Generations bought with $RAREFRIENDS at a rival's ship, every shot
is burned, and the winner keeps the loser's entire stake.

**Source**
[github.com/Halldon-Inc/pirate-friends](https://github.com/Halldon-Inc/pirate-friends), game in
[`games/pirate-friends`](https://github.com/Halldon-Inc/pirate-friends/tree/main/games/pirate-friends).
FriendSDK **v0.1.3** (CLI game directory, SDK runtime for wallet, Friend selection, ownership gate and confirmations).

```sh
git clone https://github.com/Halldon-Inc/pirate-friends && cd pirate-friends
npm ci && npm run build
npm run dev:game -- games/pirate-friends
```

## How it plays

| The harbor | Victory |
| --- | --- |
| ![Harbor](https://raw.githubusercontent.com/Halldon-Inc/pirate-friends/main/docs/media/harbor.png) | ![Victory](https://raw.githubusercontent.com/Halldon-Inc/pirate-friends/main/docs/media/victory.png) |

1. **Load the hold with one confirmation.** Pick 1 to 10 kegs. One SDK confirmation converts RF into kegs, and each
   keg cracks into 10 Generations. After that there are no more prompts, so you can fire as fast as the cannon reloads.
2. **Pick a rival and stake.** You and the rival each load the same stake: Barnacle Bess 20, Redbeard Rook 30, The
   Dread Admiral 50.
3. **Fire.** Your Friend is the captain on deck, the emblem on your mainsail, and the ammunition. Mouse direction sets
   the angle and distance sets the power; click to fire, hold for rapid fire. Keyboard: A/D angle, W/S power, Space
   fire. Touch: drag and release, or hold FIRE. Wind bends every shot, and rival ships tack back and forth.
4. **Aim for the weak points.**

| Target | Damage | Effect |
| --- | --- | --- |
| Powder magazine (glowing TNT hatch) | 40 | One blast per ship, sets the deck on fire, +5 Generations salvage |
| Waterline cracks | 15 | Leak that keeps draining hull, stacking |
| Captain's cabin | 13 | +2 Generations plunder |
| Sails | 5 | Shot rips through and keeps flying; each tear slows their reload 28%, 4 tears snap the mast |
| Hull | 9 | Solid hit |

On the way across: gulls bounce your shot higher (+1), RF barrels are trampolines (+2), treasure chests pay +5, flat
fast shots skip off the water, the Kraken eats any shot it touches, and you can shoot their cannonballs out of the
sky (+1). Hit streaks add up to +40% damage.

5. **Winner takes the hold.** Sink them or outlast their ammunition and you get back your unfired Generations, plus
   their whole stake, plus salvage. Run dry or sink and they take your whole stake. Fired shots are burned either way.

## Economy (simulated, as the rules require)

| Rule | Exact value |
| --- | --- |
| Keg price | 1 RF (`1000000000000000000` base units), SDK consumable "Powder kegs" |
| Generations per keg | 10 |
| SDK outcome table | One row, 10,000 bps, "Keg buyback reserve" worth 1 RF. The SDK requires a prize, so each keg reserves its full price. The game never calls `play`, `settle` or `redeem`, so no buyback is offered in the preview. |
| Stakes | 20, 30 or 50 Generations a side |
| Win | `+ stake - fired + salvage` Generations |
| Loss | `- stake` Generations |

**Why it is Token Activity:** Generations can only be made by spending RF, and every shot destroys one. Battles are
fast (a full Admiral fight is 50 Generations, 5 RF, a side at stake) and the only way back into a fight after a loss
is to load more kegs. In a live version each fired Generation burns its RF.

The only SDK action used is `client.buy(kegs)`: the single confirmation. The Generations ledger, stakes, payouts and
salvage live inside the game frame for the runtime session and reset on reload, labeled as simulated throughout.

## Checks

- `npx friendsdk check games/pirate-friends`: valid. `npm run check:games`: all examples and this game valid.
- `npx tsc -p games/pirate-friends/tsconfig.json`: clean.
- SDK mock-wallet browser runs at 960, 600 and 390 px: load kegs with one confirmation, start a battle, fire 6 shots
  with no further confirmation, forfeit, check the result screen. A second run aims with the mouse, sinks Barnacle
  Bess and checks the payout (40 loaded, 20 staked, 6 fired, won back 14 + 20 + 8 salvage, hold 62).
- The public preview loads with no console errors and stops at the SDK's wallet and Friend gate.

## Known issues and limits

- **Opponents are AI captains, not other players.** The SDK sandbox only allows network access to the Robinhood RPC,
  so live matchmaking is not possible in SDK v0.1.3. Real player vs player needs a match service and an escrow
  contract holding both stakes.
- **Skill results are decided in the browser.** A live version needs a server-verified or replay-verified result
  before an escrow pays out.
- **Generations are a game-local currency in the preview.** Minting, transferring and burning an RF-backed currency
  needs integration beyond the SDK's single consumable.
- Preview state resets on reload (the SDK has no save API). The automated checks use the SDK's mock wallet; the
  real ownership gate needs a wallet holding a hardwired Generation.
- No real funds move anywhere in this preview.

## Credits

All art is drawn in code for this game; the Friend is the canonical Generations sprite via FriendSDK, drawn unmodified
with a tricorn hat on top. Sound is synthesized with Web Audio. No third-party assets. FriendSDK is Apache-2.0.
