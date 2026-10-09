# Wreck Mirror (Assetto Corsa x BeamNG.drive)

**Status: design only.** Nothing is built, tested or published. Melty has not been contacted
(no `game_info`, `search_mashups` or `list_my_mods` yet), and no real screenshot exists.

## The idea
You drive in **Assetto Corsa**. A matching car in **BeamNG.drive** gets your exact steering,
throttle and brake in real time and drives with its own soft-body physics beside you. When you
crash in AC, BeamNG re-enacts the hit and its car crumples and breaks. A damage score on your
AC screen shows how wrecked the BeamNG car is.

- **Primary game:** Assetto Corsa (the player is in it; Melty installs into it and starts it).
- **Companion game:** BeamNG.drive, running at the same time (`recipe.together`). Its cars and
  physics come from the player's own install; none of its content is shipped.
- **Players:** solo first. Friends later (compare whose wreck took the most damage).

## First minute
1. Press Play once in Melty. AC opens on the left half of the screen, BeamNG on the right.
2. Pick a car in AC. BeamNG spawns the closest match on a flat test map.
3. Drive. The BeamNG car mirrors you. Crash. Watch it wreck. Read the damage score in AC.
4. Restart the session in AC (or click Re-sync): both cars go back to the start together.

## First playable must have
Core input mirror, damage score on the AC screen, car auto-matching, one-key re-sync
(AC's own restart or back-to-pits key, with an app button as fallback).

## Out of the first version
Friends and shared sessions; the same track geometry in BeamNG (the crash is a re-enactment
at the same speed, angle and zone, not the same wall); driving in BeamNG with AC mirroring;
any AC track or car content shipped inside the mod.

## Design decisions already made (technical, not asking)
- A localhost UDP bridge: an AC Python app on one side, a BeamNG game-side Lua extension on the other.
- AC's own restart key is the one-key re-sync.
- Both games borderless, side by side on one screen (or BeamNG on a second monitor).

## Files
- `sheets/` is the source of truth: games, messages, car_classes, systems, components, listing.
  Change a sheet before changing code.
- `preflight.py` lays the sheets over each other and lists unfilled cells, unverified rows and
  broken references. Last result: 0 broken references, 22 unfilled cells, 46 open items to
  verify (see `preflight-report.txt`). Build only when it reports CLEAN.

## Biggest risks to settle first
1. **One-click play.** Neither game has a loader Melty installs by itself. The AC app must be
   enabled without the player ticking anything, and BeamNG must load a map and spawn a car on
   launch. `one_click_check` decides this.
2. **AC's embedded Python and sockets.** It may need socket modules bundled, with their license checked.
3. **Crash re-enactment.** Spike barrier spawn against force impulse in BeamNG and keep the one
   that deforms convincingly.
4. **BeamNG model names** in `car_classes` are from memory and must be checked in the installed game.

## Next step
Open a coding agent on the PC with both games installed (Claude Code or Codex), give it this
folder and your Melty publish prompt, and tell it: "Continue from this design. Read game_info
and search_mashups first, fill the unfilled cells from the installed games, run preflight.py
until clean, then build, test, capture a real screenshot and publish."
