**Use it: <https://rare-friends-cards.vercel.app>** with no wallet connect, no signature and no install. Paste any wallet or ENS name.

![A live portfolio card](https://raw.githubusercontent.com/Halldon-Inc/rare-friends-cards-public/main/docs/media/card.png)

**Project name**

Rare Friends Cards + Meme Machine

**Builder / contact**

Hunt &middot; GitHub [@huntclubhero](https://github.com/huntclubhero) &middot; wallet `huntclubhero.eth`

**Category**

Character Spotlight

**One sentence**

Every Rare Friend gets a live, shareable stat card and a meme machine that puts the Friend's own on-chain art on
the face of 23 formats, and rarefriends.com's own portfolio **Share** button already sends holders here.

**Source code**

<https://github.com/Halldon-Inc/rare-friends-cards-public> &middot; Next.js 15, React 19, `next/og` (satori) for the
PNGs. **FriendSDK is not used**; this is a web tool.

**Working demo**

<https://rare-friends-cards.vercel.app> (cards) &middot; <https://rare-friends-cards.vercel.app/memes> (Meme Machine)

---

## It is already part of Rare Friends

This was built for the community before the Vibeathon and is in daily use:

- **rarefriends.com links to it.** The portfolio page's **Share** button opens
  `rare-friends-cards.vercel.app/card/<your wallet>` (`src/features/portfolio/reward-overview.tsx` in
  [rarefriends-web-public](https://github.com/spokesz/rarefriends-web-public), commit `c651683`).
- The founder shared it publicly when it launched.
- When the protocol changed its reward formula on 2026-09-20, the cards were re-derived from the protocol's own
  source the same day. The numbers are audited against those formulas (below).

## How to use it

**Cards**
1. Paste a wallet address or ENS name on the home page.
2. `/card/<wallet>` shows the whole portfolio: Friends earning and inactive, claimable RF and WETH, pending
   rewards, your APR, this week's budget, and every Friend's own art.
3. Click any Friend for its own card at `/card/<wallet>/<id>`.
4. Every card is also a 1200x630 PNG (`.../og`), so a pasted link unfurls as the card on X, Telegram and Discord.
   Download or copy it from the page.

**Meme Machine** (`/memes`)
1. Pull a Friend straight from your wallet, or drop or paste any PFP.
2. Your Friend becomes the main character: its real on-chain artwork is placed on every face slot of 20 classic
   templates, plus three formats drawn in code (Classic, Deal with it, Holo card).
3. Edit or shuffle the captions, then download or copy. Everything is assembled in your browser; nothing uploads.

No RF is spent, no rewards change hands, and no wallet is connected.

![The Meme Machine](https://raw.githubusercontent.com/Halldon-Inc/rare-friends-cards-public/main/docs/media/memes.png)

## Where the numbers come from

Every figure is read fresh from `rarefriends.com/api/protocol/state` and stamped with the block it was read at.
Every derived figure uses rarefriends.com's own formulas (`getRewardOutlook`, `holderApyPercent`,
`friendWeight` from their public repo): Your APR is the current active stream divided by the RF you paid to
activate, annualized; Pending is (stream remaining + pending) x your share of active weight.

## Checks

| check | result |
| --- | --- |
| `node scripts/audit.mjs https://rare-friends-cards.vercel.app <wallet>` | **29/29**: every figure on the live pages matches an independent implementation of rarefriends.com's formulas, read in the same second; both PNG routes render |
| `npm run build` | clean |
| Link previews | the `/og` PNGs render with bundled fonts, so previews do not depend on Google Fonts |

## Known issues and limitations

- **It depends on rarefriends.com's public API**, which is undocumented and has changed without notice (routes were
  renamed on 2026-09-19, and the APR formula changed on 2026-09-20). If it changes again, the cards follow the
  protocol's own source and the audit script catches any drift.
- PNGs are cached for two minutes for link previews, so a card can trail the chain by up to two minutes. Every card
  prints the block it was read at.
- Read-only. There are no wallets, keys or funds anywhere in the tool.

## Credits

- **Meme templates** in `public/memes/` are widely circulated internet meme images, sourced via imgflip.com. They
  are not ours; all rights belong to their original creators, and they are used only as backgrounds for holders'
  own memes: Drake, Distracted boyfriend, Two buttons, Change my mind, Expanding brain, Gru's plan, Once again asking
  (Bernie), Is this a pigeon?, Panik/kalm/panik, Buff doge vs cheems, Trade offer, Always has been, This is fine,
  Surprised Pikachu, Woman yelling at cat, Hide the pain Harold, Draw 25, Tuxedo Pooh, Monkey puppet, Left exit 12.
- **Fonts:** Silkscreen (Jason Kottke) and Sometype Mono (Dharma Type), SIL Open Font License 1.1.
- **Friend artwork** is each NFT's own on-chain SVG, read from rarefriends.com and rendered unmodified.
