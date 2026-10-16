# Changelog

## 1.975-fix1

Fixes for item lookup and swapping on a Turtle-like 1.12 server. Only `ItemRack.lua` changes.

### Item lookup
- A set item id contains its enchant (`ID:enchant:suffix`). Matching is now: exact id first, then the same item with another
  enchant, then name. The saved id follows the item when it is re-enchanted.
- `IsSetEquipped` and `EquipSet` accept an item worn with another enchant; if everything is already worn the set becomes the
  current one without any swap.
- The item name falls back to the item link when `GetItemInfo` returns nothing (item not in the client cache yet).

### Bank
- Banked items are tracked by slot and id, not by name: two copies of the same item are both found and both retrievable.
- `PLAYERBANKSLOTS_CHANGED` is handled, so the list refreshes after moving items in the 24 main bank slots.
- A set can be equipped from the bank while it is open; with the bank closed, the "could not find" message tells you to open it.
- Clicking the "(empty)" entry no longer calls `PickupContainerItem(nil, nil)`.
- Storing a set in the bank leaves the worn pieces on the character.

### Swap queue
- The queue is emptied completely when swapping stops (it used to skip every other entry, replaying a stale swap on the next set).
- An entry that moves nothing no longer waits for an item-lock event; a 1.5 s watchdog unblocks a stuck swap (abandoned after about 30 s of locked item).
- After a set is applied it is verified: up to 2 automatic retries, then `ItemRack: could not equip: ...`.

### Menu
- Before equipping a clicked entry, the saved (bag, slot) is checked; if the slot changed the item is found by name or the menu is rebuilt.
- With the bank open, Ctrl+click equips normally; a plain click still moves the item to or from the bank, as in the original.

### Misc
- `"^Rack-"` → `"^Rack%-"` (the dash was a Lua quantifier, so names starting with "Rac" were treated as internal sets).
