<p align="center"><img src="logo.svg" width="96" alt="Give It Back: Dice Battles logo"></p>

# Give It Back: Dice Battles

A collectible dice battler in a single HTML file, set in the world of
Give It Back: The RPG. Collect dice in eight colours, build an army of up to
six, line it up in two ranks and roll for it.

It is free, with no ads, purchases or sign-up.

## Play

Open `index.html` in any browser. There is nothing to install or build.

To put it online, turn on GitHub Pages for this repository (Settings → Pages →
deploy from the `main` branch, root folder).

Your collection, coins, saved armies and settings are kept in the browser.

## What is in it

- **400 dice.** Eight colours, each with 10 monsters, 10 named heroes and
  30 commons such as the Red *Street Fighter Orc*.
- **Colours.** Every colour has a power (Blaze, Chill, Regrowth, Bulwark,
  Spark, Blessing, Leech, Pierce) and beats one other colour. Red beats Green,
  Green beats Brown, Brown beats Yellow, Yellow beats Blue, Blue beats Red;
  White beats Black, Black beats Purple, Purple beats White. A die rolls an
  extra attack die against the colour it beats.
- **Heroes** combine one of 6 races (Human, Dwarf, Elf, Orc, Halfling,
  Golemkin) with one of 6 classes (Warrior, Rogue, Mage, Cleric, Ranger,
  Paladin). Each race has a perk and each class an ability.
- **Monsters** come in 12 kinds, from goblins and skeletons to golems,
  gazers and legendary dragons.
- **Dice combat.** The attacker rolls attack dice, the defender rolls defence
  dice, and the difference is the damage. Front and back ranks, cooldowns,
  poison, stuns, hexes and guards.
- **Modes.** Battle the computer (Easy, Normal, Hard), two players on one
  device, or watch AI vs AI with pause and speed controls.
- **Collecting.** You build every army from your own portfolio. Pick a home
  realm to start with 13 dice and 200 gold, then earn the rest:
  - Gold for every battle against the computer (40, 70 or 110 for a win by
    difficulty, 15 for a loss), 20 for a two-player game, 100 for finishing
    the walkthrough.
  - Spoils of war: after a win, claim one die from the army you beat.
  - A market of single dice that restocks after every battle, plus mixed and
    colour packs. Sell spare copies back for gold.
- **Portfolio.** A dice album grouped by realm with set progress, a guide
  page for every die (what it does, a short history, a line of fluff and its
  battle record), your saved armies with win–loss records, and a realms guide
  to the eight colours and the story of the Great Die.
- **Easy to learn.** Hover any die for its details (tap ⓘ on touch screens).
  The walkthrough and a builder tour spotlight each part of the screen. While
  you build, your army panel and Start button follow you down the list.
- **Settings.** Light, dark or automatic theme, army points, dice and
  computer speed, damage hints, sound, motion, text size and an
  "all dice unlocked" switch.

## Controls

| Action | Key |
| --- | --- |
| Attack with the selected die | A |
| Use its ability | S |
| Next ready die | Tab |
| End turn | E |
| Cancel or close | Escape |

Click a die to select it, then click a glowing target.

## Files

- `index.html`: the whole game: markup, styles, data, rules and AI
- `logo.svg`: repository logo and page icon
- `LICENSE`: MIT licence

## License

MIT. See `LICENSE`.
