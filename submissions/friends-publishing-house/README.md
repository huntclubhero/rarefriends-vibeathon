![Friends Publishing House](https://raw.githubusercontent.com/Halldon-Inc/friends-publishing-house/master/docs/media/home.png)

# Friends Publishing House

**Your Friend. Your manga.**

**Live:** https://friends-publishing-house.vercel.app
**Read the demo issue (no wallet needed):** https://friends-publishing-house.vercel.app/read/the-gm-heist-2fe169

**Project**
Friends Publishing House, a manga studio and publishing shelf for Rare Friends holders, built with FriendSDK v0.1.3.

**Builder / contact**
Hunt &middot; GitHub [@huntclubhero](https://github.com/huntclubhero) &middot; wallet `huntclubhero.eth`

**Category**
Character Spotlight

**One sentence**
Holders build worlds with FriendSDK, cast their own Rare Friends as the leads, ink manga pages in black and white or
full colour, and publish issues anyone can read, share to X, embed, or remix.

**Source**
[github.com/Halldon-Inc/friends-publishing-house](https://github.com/Halldon-Inc/friends-publishing-house)
(Next.js 15, TypeScript). FriendSDK v0.1.3 is vendored as its release tarball (`vendor/`, SHA-256 matches the
release's SHA256SUMS).

```sh
git clone https://github.com/Halldon-Inc/friends-publishing-house && cd friends-publishing-house
npm install
cp .env.example .env.local
npx next dev -p 3190
```

## Why it is Character Spotlight

Your Friend is the main character of every page. Generations Friends are drawn from **FriendSDK's canonical
sprites** (`createGenerationSpriteReader`), so a creator can pose them in **4 facings, idle or walking, across 8
animation frames**, the same artwork the SDK games use. Genesis Friends use their on-chain `tokenURI` portrait.
The cast picker lists the Friends in your wallet, and any Friend can guest star by number.

| Worlds, in colour | ...or black and white |
| --- | --- |
| ![World builder in colour](https://raw.githubusercontent.com/Halldon-Inc/friends-publishing-house/master/docs/media/world-colour.png) | ![World builder in B&W](https://raw.githubusercontent.com/Halldon-Inc/friends-publishing-house/master/docs/media/world-bw.png) |

## How to use it

**Reading (anyone):** open the shelf on the home page, pick an issue, read it scrolling or page by page. Share
buttons post to X, Farcaster or Telegram, copy the link, or copy an iframe embed. Each page downloads as a PNG, and
there is an RSS feed at `/feed.xml`.

**Creating (holders):**
1. **Sign in.** Open `/studio`, connect a wallet holding a Genesis or Generations Friend on Robinhood mainnet, and
   sign one plain-text sign-in message (EIP-4361). It is free and sends no transaction; the server verifies the
   signature and checks `balanceOf` on both collections.
2. **Build a world.** Start from any of the six FriendSDK worlds (Garden Commons, Circuit Courtyard, Crystal Steps,
   Rooftop Hangout, Tidal Islands, Orbital Array), drag in any of the 18 SDK props, walk your Friends onto the
   ground, and switch between **black and white** (SDK monochrome with signal green) and **colour** (SDK
   `GAME_PALETTE`). Rendering is the SDK's own `renderWorld`, with Friends passed as live actors so they depth-sort
   among the props, and `worldContains` keeping everything on the ground.
3. **Ink the pages.** 9 panel layouts (slanted manga cuts, 4-koma, splash, grid), screentones, speed lines, aimable
   focus lines, worlds as panel backgrounds you pan and zoom, speech/shout/thought/whisper/narration bubbles with
   draggable tails, SFX lettering (GM, LFG and WAGMI included), manga emotes and SDK prop stickers. Undo, autosave,
   and one switch turns the whole issue B&W or colour.
4. **Publish.** Pages render in the browser from the same SVG the editor shows (1200 x 1800 PNG) plus a
   1200 x 630 share card, so a link posted on X unfurls with the cover. Republishing keeps the link.
5. **Remix.** Any published issue has a "Remix this issue" button: a holder gets a copy with their own Friends to
   swap in, and the published remix credits the original.

![The GM Heist, a 3-page demo issue](https://raw.githubusercontent.com/Halldon-Inc/friends-publishing-house/master/docs/media/gm-heist-pages.png)

| Editor | Share card on X |
| --- | --- |
| ![Editor](https://raw.githubusercontent.com/Halldon-Inc/friends-publishing-house/master/docs/media/editor.png) | ![X card](https://raw.githubusercontent.com/Halldon-Inc/friends-publishing-house/master/docs/media/x-card.png) |

**RF costs and rewards:** none. Creating, publishing and reading are free; there are no purchases, simulated or
real, and no contract calls beyond read-only ownership checks.

**Third-party assets:** world, prop and character artwork via FriendSDK (Rare Friends Isometric World Assets,
canonical Generations sprites; see FriendSDK NOTICE.md) and on-chain Genesis portraits. Fonts under the SIL Open
Font License: Dela Gothic One, Bangers, Comic Neue, Silkscreen, Space Grotesk. Everything else (tones, bubbles,
emotes, layouts) is drawn in code.

## Checks and known issues

- **Checks run:** TypeScript typecheck clean; `next build` green. A Playwright end-to-end run covers sign-in, building a
  world, editing pages, publishing, the reader and its `og:image` / `twitter:card` tags, with 0 console errors and
  no horizontal overflow at 390 px. After deploy, a production smoke test confirmed pages load, studio APIs refuse
  requests without a holder session, and the local-only dev sign-in returns 404.
- **Not yet verified:** a full sign-in and publish with a real wallet on the live site. The flow above was tested
  locally with a development-only sign-in.
- **MetaMask warning:** on the new `*.vercel.app` domain, MetaMask's security alerts have shown a "malicious"
  warning on the sign-in signature. The message is a valid EIP-4361 sign-in whose domain matches the site, it is on
  no phishing list we checked, and signing it cannot move funds. We believe it is a reputation false positive for a
  new domain.
- **Moderation:** there is no report button yet; published issues go straight to the public shelf. Takedowns are
  manual for now.
- **Demo issue:** "The GM Heist" is a house demo (pen name "FPH Demo Desk") made in the studio so judges without a
  Friend have something to read; its Friends appear as guest stars.
- Community project, not affiliated with Rare Friends.
