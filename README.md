# ItemRack (Turtle WoW / OctoWoW fixes)

Equipment sets and context menus for WoW **1.12** (vanilla, Turtle WoW / OctoWoW).

This is a fork of [Einherjarn/ItemRack](https://github.com/Einherjarn/ItemRack), itself a custom version of
the original **ItemRack by Gello**. It keeps the on-use trinket queue of the upstream fork and fixes the way items are
found and swapped. All credit for the addon goes to the original authors; this fork only carries the bug fixes below.

> The upstream repository has no license file. This fork is published for personal use and to share with friends,
> in the same spirit as upstream. If you are one of the original authors and want something changed, open an issue.

## What is fixed in this fork

Short version (details in [CHANGELOG.md](CHANGELOG.md)):

- **Items with a different enchant are recognised.** A set stores `id:enchant:suffix`; when you re-enchant a piece the set used to lose it (found by name only, set never "equipped", pointless swaps). Items are now matched by exact id, then same item with another enchant, then name.
- **Bank:** items are tracked per slot and id (two copies of the same item no longer collide), the bank list refreshes when you move things in the 24 main slots, sets can be equipped from the bank while it is open, and the "(empty)" menu entry no longer raises a Lua error.
- **Swap queue:** it is flushed correctly when full bags interrupt a set (a stale swap could be replayed on the next set), no longer waits forever for a lock event that never comes, and has a watchdog plus a verify-and-retry step with a clear message when something cannot be equipped.
- **Menu:** stale (bag, slot) positions are re-checked before equipping what was clicked.
- Items not yet in the client cache (`GetItemInfo` returns nil) are found through the item link.
- Pattern fix: `"^Rack-"` now escapes the dash.

Nothing changes in the saved variables format.

## Install

1. Download the repository (Code > Download ZIP).
2. Rename the folder to `ItemRack` and put it in `Interface\AddOns\`.
3. Restart the game or `/reload`.

Tip: with the bank open, a plain click on a bag item moves it to or from the bank (as in the original); use **Ctrl+click** to equip.

## Tests

The fixes were checked outside the game with a simulated world (bags, bank, cursor, locks, latency, server refusals).
These tests cannot reproduce every 1.12 server behaviour: please report anything odd with the exact Lua error text.

## Credits

- **Gello** — original ItemRack
- **Einherjarn** — Turtle WoW fork with the on-use trinket queue (upstream)
- **ShikawaLePaladin** — fixes in this fork
