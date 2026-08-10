# Lineout Moves

An animated 5-man lineout playbook. One page, no build step, no dependencies,
works offline.

**Live:** https://YOUR-USERNAME.github.io/lineout-moves/

## What's in it

Eleven calls — red / yellow / black, both bounces and black option, with green
and orange finishes. Each one animates the walk-in, the set, the throw, the lift
and the finish.

| Control | Does |
|---|---|
| ‹ › | Previous / next call |
| Call name | Opens the full list of 11 calls |
| Dots | Jump straight to a call |
| Swipe / drag on the pitch | Previous / next call |
| Tap the pitch | Pause / play |
| REPLAY · 1× / 0.5× | Playback |
| FLIP ⇅ | Throw at the bottom or the top |
| DYNAMIC / SPACES | The two walk-in methods |
| NOTES (or swipe up) | Coaching points for that call |
| Scrub bar | Drag to freeze on any phase |

## Deep links

Share a specific view:

- `#7` — open on call 7
- `#top` — throw at the top
- `#spaces` — SPACES walk-in
- `#top,spaces,7` — all three

## Reading the picture

Teal disc, scaled up, is the jumper. Teal ring is a lifter. Grey is a man who
has stepped out of the line. Hollow discs are the opposition. Green ring is 9
or 10. The orange block is the maul.

## Editing a call

Everything lives in the `<script>` block in `index.html`. `SHAPES` holds the
pods (three numbers, the middle one jumps), `SIDES` holds who fills the top and
bottom of each drive, and `MOVES` is the deck. A new call is a data change, not
a code change.

Full spec: [`docs/lineout-spec.md`](docs/lineout-spec.md).
