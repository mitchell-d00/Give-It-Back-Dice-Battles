# Handoff: Give It Back: Dice Battles

Status as of October 13, 2026. This note is for whoever picks the project up next, whether that's a person or a new AI chat.

## What the game is

Dice Battles is a collectible dice battle game in a single HTML file. It runs offline in any browser with nothing to install.

- **The dice:** 8 colours. Each colour has a power and the advantage over one other colour (it rolls 1 extra attack die against it). Each colour has 10 monsters, 10 heroes (race and class combos) and 30 commons, for 400 dice. There are also 10 secret grey Mercenaries that unlock through achievements.
- **Armies:** 6 dice in two ranks. Melee dice fight from the front. Ranged dice fight from either rank. A melee die in the back waits as a reserve and steps up when a front die falls.
- **Combat:** the attacker rolls attack dice, the defender rolls defence dice, and the difference is the damage. Abilities have cooldowns, and there are statuses such as poison, stun, hex, chill and guard.
- **Collecting:** new players name their Dice Lord and pick a home realm. They get 10 dice from it, a Traitor Pack of 5 dice from other colours and 200 gold. After that they earn gold and dice by winning: spoils from beaten armies, a market, and packs.
- **Levels:** 1 to 10, bought with gold. Dice evolve at levels 4, 7 and 10 (Veteran, Elite, Legend), and each evolution gets a new name and a new story chapter.
- **Modes:**
  - Face a rival Dice Lord: named rivals with a grudge. You can view their army but not change it.
  - Practice battle: you build both armies and set the computer's dice level. No rewards.
  - Two players on one device.
  - AI vs AI: two named rival lords.
  - Walkthrough.
- **Other features:** a Portfolio with an info guide, lore and history for every die; a world history timeline; light and dark themes; sticky army panel; hover info; mobile action bar.

**Owner:** Mitchell D. McPhetridge. MIT licence. Based on his earlier game, Give It Back: The RPG.

## Where things are

| What | Where |
|---|---|
| Live game (artifact, version 7) | https://claude.ai/artifact/MKdJoKho5Yh4wyWCseSEgj |
| Source of truth | `game.html` (an HTML fragment that starts at `<title>`, with no doctype) |
| Repo | `Give-It-Back-Dice-Battles/`: `index.html` (game.html wrapped in a full HTML page with favicon), `README.md`, `LICENSE`, `logo.svg`, this file |
| Source pieces (already inlined into game.html) | `lore.js`, `extra.js`, `smart.js`, `mid.js`, `lords.js` |
| Backups | `game_before_lords.html`, `game_before_portfolio.html`, `game_before_levels.html`, `game_before_smart.html`, `game_before_ranks.html` |

Save data lives in `localStorage` under the `gib-dice.` prefix. Every read and write goes through a try/catch wrapper called `store`. The player profile is the object `P`; gold is `P.coins` and owned dice are `P.col`.

## How the code is organised (inside game.html)

- **Data:** `COLORS` (each with `beats`), `CLS`, `RAC`, `ARCH`, `JOBS`, `HEROES`, `MONSTERS`, `DICE` and `DIE` (lookup by id), `MERCS`, `HISTORY`, `MYTH`, `REALM`.
- **Die stats and art:** `eff(d,lv)` (effective stats at a level), `dname(d,lv)` (evolved name), `tileSVG` and `tileLv` (art), `lore(d)` and `chapters(d)`.
- **Strength and computer armies:**
  - `rating` and `armyPower` estimate strength.
  - `smartArmy(o)` builds an army with a front line, ranged support and colour counters.
  - `computerArmy(playerArmy,diff)` scales to the player's strength (Easy 0.85x, Normal 1x, Hard 1.08 to 1.15x) at the player's average level.
  - `aiPair` builds two matched armies for AI vs AI.
  - `tuneLevels` raises or lowers levels to hit a target strength.
- **Dice Lords (`lords.js`):** `makeRival(diff)`, `setRivalRealm(rival,army)`, `lordName()`, `homeRealm()`, `GRUDGES`, `RIVAL_WIN`, `RIVAL_LOSE`.
- **Builder:**
  - `startMode(mode)`, where mode is one of `cpu`, `custom`, `pvp`, `aivai` or `tutorial`, and builds `BU`.
  - `renderBuilder()`, `placeRow`, `rankAdvice`, and Auto-arrange (`#bArrange`).
  - `locked()` is true on the rival's army screen. It hides the editing tools, every edit handler checks it, and it shows the `#challenge` card.
- **Battle:**
  - `startBattle(cfg)` builds the battle state `B`.
  - Turn logic: `beginTurn`, `endTurn`, `canAct`, `basicT` and `abilT` (who can be targeted), `computeHit`, `advance()` (reserves step up).
  - Results: `endBattle` (rewards, spoils, stats; practice mode gives none).
  - Battle UI: `renderOrders`, `renderDock`.
- **Walkthrough:** the `coach` object (spotlight steps), `startTutorial()`, `startBuilderTour()`.
- **Collection screens:** `renderCollection`, `renderAlbum`, `renderArmies`, `renderRealms`, `showDie` (guide page with the upgrade button), plus the market and shop.

## How to work on it

1. Edit `game.html`. For large changes, write a Python patch script that asserts each search string appears exactly once before replacing it.
2. **Syntax-check every time, including after one-line edits.** Pull the `<script>` blocks out to a `.js` file and run `node --check` on it. Version 6 went out broken: a `${...}` was placed inside a single-quoted string, and the game would not load at all. It was fixed in version 7.
3. Browser-test with Playwright: `NODE_PATH=$(npm root -g) node test.js`. Wrap game.html in a doctype page and add `[hidden]{display:none!important}` to that test page.
4. Publish `game.html` to the same artifact URL.
5. Rebuild the repo: put game.html into `index.html` from `<title>` up to the old `</body>`, then zip the folder.

## What changed most recently (October 14)

- **The Court of Dice Lords** replaced Easy/Normal/Hard in player vs computer (`court.js`, inlined). `newCourt(season)` makes 21 random lords in six ranks (`COURT_TIERS`: strength 80, 108, 152, 204, 265, 355). The court is saved in `P.court` and regenerated when the portfolio is reset.
  - Lord armies: `lordArmy(L, playerArmy)` builds from the lord's style pool and sizes the army to `lordPower(L)`. Hedge Lords may bring 4 or 5 dice.
  - Results: `lordResult(winner)` handles gold, unlocks, the crown and the next challenger.
  - Screen: `renderCourt()` draws `#scr-court`.
- **Balance (AI-played simulations):** a sensibly built starter army beat the Hedge Lords 12/12 and the Banner Lords 9/12. Equally rated armies won about 75% against higher ranks, so ranks from Landed up were raised about 8%.
- **Fixes:** a healer no longer counts as ready when nobody is hurt, so the turn ends by itself. The walkthrough has a 4th goblin, and its card sits first on the menu until it's done.

## Earlier changes

- **Dice Lords:** the player names their lord when they start, and can rename it in Settings.
- **Rival lords:** each rival gets a name, a holding, a rank by difficulty and a grudge. Rivals taunt the player on the result screen.
- **Locked rival armies:** a rival's army can be viewed but not edited. The buttons are "Change my army" and "Accept the challenge".
- **Practice battle:** you build both armies and can set the computer's level from 1 to 10 with a slider. No gold, spoils or records.
- **AI vs AI:** now shows two named rival lords.
- **New lore and docs:** a "The Dice Lords" history entry, and updates to How to play and the README.

## Playtest findings (October 13, an unfinished first play as a new player)

**Update, October 14:** items 2, 4, 5 and 6 are addressed. The walkthrough card comes first until it's done; the walkthrough has a 4th goblin; Hedge Lords are weaker than a starter army; healers no longer block the turn. Item 3 (naming before the walkthrough) and item 7 are still open.

I played through the UI at normal speed with real clicks. These came up:

1. **Fixed:** version 6 would not load. It's fixed in the live artifact and in the repo zip.
2. **The walkthrough is the last card on the menu.** It should come first, or stand out, for a player with no dice.
3. **The walkthrough runs before the player names their lord,** so the battle calls them "You". Consider moving lord naming and realm choice ahead of the walkthrough.
4. **The walkthrough battle is too easy.** It ended in round 2 after a single action of the player's own. The coach's last step says "Finish this fight", and then it's already over. Give the goblins more health, or one more goblin.
5. **The first real battle was a blowout.** The builder rated it "An even match" (104 vs 109). The new player's army was simply the first dice in the list. It lost 0/6 against 5/6 in 4 rounds (about 45 seconds). That's one sample, but it suggests the strength rating doesn't capture how bad an unorganised army is, or that the computer's colour counters hit harder than the rating thinks. Run more simulations of a new player's starter army against Normal rivals.
6. **A die the player couldn't use.** Twice, a die the game counted as "ready" (uid 0-4) couldn't be ordered, and the player had to press End turn by hand. `canAct` returns true when the die has an ability with targets, but the attack and ability buttons may not line up with that. Look into it, and end the turn automatically when no ready die has a usable action.
7. **New players face a long setup:** a starter modal, two pack reveals, and a six-step builder tour before they can build anything. Consider shortening the tour to three steps.
8. **Not yet tested:** the shop, buying, upgrades, the portfolio, the phone layout, dark mode, two-player mode and Mercenary unlocks. Those are the next things to play.

## Suggested next steps

1. Fix findings 2 to 6 above.
2. Run a batch of simulations of a new player's starter army against Easy and Normal rivals, and retune if they lose most of the time.
3. Finish the playtest: shop, upgrades, portfolio, phone layout, dark mode.
4. Bigger idea: rivals who remember you, with rematches, escalating grudges and a small map of realms to conquer.

## Owner's preferences

- Single file that works offline, with light and dark themes and plain-language help.
- Social-media blurbs should be humble, in Mitchell's own voice, and explain the game. Never credit AI.
- Expect to be asked to test by actually playing, not by reasoning about the code.

## Related project

Mitchell D Chess (a separate artifact, https://claude.ai/artifact/1TmK4ecSzt2QoqmQR1GoeN), in `/home/claude/chess/`:

- A single-file chess game with 35 AI levels and a 38-lesson tutorial.
- Modes: player vs player, player vs AI, and AI vs AI.
- Move timers and an official FIDE rules mode.
- Colour pickers for the board and pieces.
- The engine is in `engine.js`, verified with perft.
