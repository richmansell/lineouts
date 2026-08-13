# Lineout Moves — Single Page Web App

**Spec v0.9 — built. 10 Aug 2026**

v0.9 = black option receiver reroutes up past the tail before running forward.
v0.8 = KIWI added — two dummies then a front-ball strike.
v0.7 = RED GREEN removed; four-man lineout added (6 calls); the 10 renamed **+1**
throughout; the thrower joins every four-man drive.
v0.6 = 10 dropped behind the 9 (no forward pass); black option 1 runs onto the
ball; black option carry renamed GREEN.
v0.5 = proper call navigation (prev/next, call list, tappable dots, mouse-drag swipe).
v0.4 = 10 moved out beyond the tail, thrower unnumbered. v0.3 = app built. Changes since v0.2: direction vocabulary
pinned down, drive sides finalised per call, red bounce yellow confirmed at the
YELLOW station, black option finishes confirmed, 9 repositioned, opposition
timing set, SPACES made a toggle.

Deliverable: **`lineout.html`** — one file, no dependencies, works offline.

---

## 1. Direction vocabulary

The single biggest source of confusion, so it is fixed here and used everywhere.

| Term | Means | Default screen position |
|---|---|---|
| **TOP** (of the drive) | The **throw end** — front of the lineout, 5m | Bottom of the screen |
| **BOTTOM** (of the drive) | The **far end** — back of the lineout, 15m | Top of the screen |
| **Front / back of the lineout** | Nearest / furthest from the thrower | Bottom / top |
| **Attack, drive** | Toward the opposition line | Right |

So on the default view, "top of the drive" is drawn at the bottom of the screen.
That's the playbook's language, kept as-is.

---

## 2. Pitch model

Touchline along the bottom, line of touch running up the screen, us on the left,
opposition on the right, attack to the right. viewBox `0 26 100 154`.

| Actor | x | y |
|---|---|---|
| Thrower (**T** — joins every four-man drive) | 54 | 170 |
| Touchline | — | 160 |
| **1** — front man (5m) | 42 | 140 |
| **2** — RED station | 42 | 118 |
| **3** — YELLOW station | 42 | 96 |
| **4** — BLACK station | 42 | 74 |
| **5** — back man (15m) | 42 | 52 |
| Opposition ×5 | 68 | mirrored |
| 9 | 19 | 94 (near yellow, off to the left) |
| **+1** | 6 | 50 (beyond the tail **and behind the 9** — the pass has to go back) |

Pod members close to ~12 units (≈1.4m) apart against the 22-unit station spacing.

### Four-man lineout

Same pitch, four in the line. **1 and 4 hold, 2 and 3 walk in.**

| Man | y | Station |
|---|---|---|
| **1** | 134 | — |
| **2** | 110 | **Y1** |
| **3** | 86 | **Y2** |
| **4** | 62 | — |

Opposition drop to four as well. The black option jump sits at **74 (B)**, at
the back.

### Orientation toggle

Both orientations ship. Not a mirror — flipping y is the same lineout on the
**other touchline**: we stay left, opposition stay right, attack still goes
right. One function, `Y = y => flip ? (206 - y) : y`, applied to every y at draw
time. Text sits outside the flip so nothing reads upside down. Move data is
authored once. **Default: throw at the bottom.**

---

## 3. Notation

Our men are numbered front to back. In a five-man line, 1 and 5 are static and
2, 3, 4 walk in; in a four-man line, 1 and 4 are static and 2, 3 walk in.

The extra forward who comes in — or stands out as the receiver on a green — is
shown as **+1**, never as a shirt number.

> **A pod is three numbers and the middle one jumps.** The outer two lift.

| Call | Dummy pod | Real pod | Jumper | At |
|---|---|---|---|---|
| RED | — | **1 · 2 · 3** | 2 | RED |
| YELLOW | — | **2 · 3 · 4** | 3 | YELLOW |
| BLACK | — | **3 · 4 · 5** | 4 | BLACK |
| RED BOUNCE YELLOW | 1 · 2 · 3 at RED | **1 · 2 · 4** | 2 | **YELLOW** |
| YELLOW BOUNCE RED | 2 · 3 · 4 at YELLOW | **1 · 3 · 4** | 3 | **RED** |
| BLACK OPTION | — | **+1 · 4 · 5** | 4 | BLACK |
| 4 MAN YELLOW 1 | — | **1 · 2 · 3** | 2 | Y1 |
| 4 MAN YELLOW 2 | — | **2 · 3 · 4** | 3 | Y2 |
| 4 MAN BLACK OPTION | — | **+1 · 3 · 4** | 3 | B (at the back) |
| KIWI | 1 · 2 · 3 at RED, then 1 · 3 · 4 at YELLOW | **2 · 3 · 4** | 3 | RED |

A bounce is **one station's shift**, and the man who showed for the dummy is the
man who jumps. The third man of the dummy clears out to the left.

---

## 4. The deck — 17 moves

**GREEN** = off the top; ball goes to **9**, who passes back out to **10** standing
beyond the tail of the lineout — unless the call states otherwise.
**ORANGE** = drive. Black is only driven as BLACK OPTION.

**RED GREEN is not in the deck — it is never done.**

| # | Call | Finish |
|---|---|---|
| | **Five man** | |
| 1 | RED | ORANGE |
| 2 | YELLOW | GREEN |
| 3 | YELLOW | ORANGE |
| 4 | BLACK | GREEN |
| 5 | RED BOUNCE YELLOW | GREEN |
| 6 | RED BOUNCE YELLOW | ORANGE |
| 7 | YELLOW BOUNCE RED | GREEN |
| 8 | YELLOW BOUNCE RED | ORANGE |
| 9 | BLACK OPTION | GREEN |
| 10 | BLACK OPTION | ORANGE |
| 11 | KIWI | GREEN |
| | **Four man** | |
| 12 | 4 MAN YELLOW 1 | GREEN |
| 13 | 4 MAN YELLOW 1 | ORANGE |
| 14 | 4 MAN YELLOW 2 | GREEN |
| 15 | 4 MAN YELLOW 2 | ORANGE |
| 16 | 4 MAN BLACK OPTION | GREEN |
| 17 | 4 MAN BLACK OPTION | ORANGE |

---

## 5. The drive

> **Every drive is the jumping pod, a man pushing in on each side, and the
> additional man taking the ball in the middle.**

The additional man is the **+1** — a forward on any orange. On either black
option he is the original **1**, who has already stepped out of the line.

**On every four-man drive the thrower comes in and drives**, taking the top.
That is what fills the second side, since a four-man line only leaves one free
man.

| Call | Pod | Top (throw end) | Bottom (far end) | Ball in the middle |
|---|---|---|---|---|
| RED | 1 · 2 · 3 | **4** | **5** | +1 |
| YELLOW | 2 · 3 · 4 | **1** | **5** | +1 |
| RED BOUNCE YELLOW | 1 · 2 · 4 | **5** — leaves early | **3** | +1 |
| YELLOW BOUNCE RED | 1 · 3 · 4 | **2** | **5** | +1 |
| BLACK OPTION | +1 · 4 · 5 | **2** | **3** | 1 |
| 4 MAN YELLOW 1 | 1 · 2 · 3 | **thrower** | **4** | +1 |
| 4 MAN YELLOW 2 | 2 · 3 · 4 | **thrower** | **1** | +1 |
| 4 MAN BLACK OPTION | +1 · 3 · 4 | **thrower** | **2** | 1 |

**Red bounce yellow is the only one where 5 makes the long trip.** He is
uninvolved from the start, so he leaves early and gets to the top, near the
throw line, to drive from that side; 3, who was in the dummy, can't leave that
early so he joins at the bottom. On yellow bounce red the jump is already at the
front, so 5 simply joins at the bottom and 2 — out of the dummy — takes the top.

---

## 6. Phase timeline

| # | Phase | What happens |
|---|---|---|
| 0 | **Walk in** | 2, 3, 4 come in from behind the line on our left. Two methods — §7. |
| 1 | **Set** | Still. Opposition group up as our pod forms. |
| 2 | **Move** | *Bounces and black option only.* Dummy shape and show, clear-out, early runner. |
| 3 | **Throw** | Ball leaves on an arc to the target. |
| 4 | **Lift** | Lifters dip and drive, jumper rises. Overlaps the throw. |
| 5 | **Catch** | Ball meets hands at the top. |
| 6 | **Finish** | GREEN: down to 9, out to 10. ORANGE: side men and the ball carrier are already there, bind, maul drives right. |
| 7 | **Hold** | Freeze on the end picture. |

Free men leave as the ball is thrown so they arrive as the jumper goes up — then
connect and drive as a unit. The early runner on red bounce yellow leaves in
phase 2.

---

## 7. Setup — SPACES and DYNAMIC

The three always come in from behind the line, on our left.

- **SPACES** — all close, get in, wait, spread to their stations, then the move
  starts.
- **DYNAMIC** — start in their relevant positions, in quick, straight into the
  move.

SPACES can be called, but in the app it's a toggle so any call can be seen
either way.

---

## 8. Opposition

Five defenders. Their front and back men hold. Their middle three group opposite
where we are going to lift, **as our pod forms**. On a bounce they group at the
**dummy** and stay there — they do not follow the bounce, which is the whole
point of the move. No contest in v1.

---

## 9. Black option — five man

1. The **+1 is a forward** and comes into the line.
2. **1 moves out first**, so it is legally a five-man lineout — 2, 3, 4, 5 + the +1.
3. 1 takes the receiver role out in the channel.
4. **4 jumps**, lifted by the **+1** (top) and **5** (bottom).
5. **1 moves out early — straight out**, staying level with the front of the
   lineout. Not out and up.
6. As the jump is made he **runs up**, so he arrives onto the ball dynamically
   rather than standing waiting for it.
7. **GREEN** — off the top to 1 on the move. He runs **up past the tail of the
   lineout**, outside their defence, then straight forward for the try line.
   He does not cut back across the front — that runs him straight into them.
8. **ORANGE** — the same run, but he arrives into the middle of the maul with the
   ball. 2 joins the top, 3 the bottom.

---

## 9a. Black option — four man

1. **1 and 2 go together at the front.**
2. **1 then steps out early — straight out** — keeping it legally four in the
   line once the +1 arrives.
3. The **+1 comes in at the back**. Pod is **+1 · 3 · 4**, so **3 jumps**,
   lifted by the +1 at the top and 4 at the bottom.
4. As the jump is made, 1 **runs up** onto the ball.
5. **GREEN** — he takes it on the move, runs **up past the tail** to get outside
   their defence, then straight forward for the try line.
6. **ORANGE** — he arrives with the ball into the middle of the maul; the
   thrower drives at the top and 2 joins at the bottom.

---

## 9b. KIWI — five man

The long one. Two dummies, a swap in the line, and a front-ball strike into the
gap between the front man and the thrower.

1. **1 · 2 · 3 dummy at the front** (RED). 2 shows.
2. **2 steps out** of the line to the left.
3. **1 · 3 · 4 dummy in the middle** (YELLOW). 3 shows.
4. **2 comes back in to the 1 position** at the front of the line.
5. **1 steps out.**
6. **Pod is 2 · 3 · 4, lifting at RED** — 3 jumps, with 2 in front of him and 4
   behind.
7. **1 takes the ball dynamically in the gap between RED and the thrower** and
   runs for the try line.

The opposition follow both dummies — they group at RED, shift to YELLOW, and are
left there when the ball goes to the front. That's the whole point of the call
and the animation makes it obvious.

No orange variant is defined. If you want one it's a data change.

---

## 10. The app

Single HTML file, portrait, SVG, no dependencies, no browser storage. Works
offline and can be added to the home screen.

- **&#8249; and &#8250; buttons** either side of the call name — previous / next call.
- **Tap the call name** — opens the full list, grouped into five-man and
  four-man, pick any one directly.
- **Dots** under the title are tappable and show position in the deck.
- **Swipe left / right** on the pitch also changes call (mouse drag works too).
- **Tap the pitch** — pause / play. **Swipe up** or **NOTES** — coaching points.
- **REPLAY**, **1× / 0.5×**, **FLIP ⇅**, **DYNAMIC / SPACES**.
- **Scrub bar** with the phases as ticks — drag to freeze on any phase.
- **Deep links** for sharing a particular view: `#top`, `#spaces`, `#7`, or
  combined — `lineout.html#top,spaces,7`.

Reading the picture: teal disc, scaled up, is the jumper; teal ring is a lifter;
grey is a man who has stepped out of the line; hollow discs are the opposition;
green ring is 9 or 10; the orange block is the maul.

---

## 11. Data model

```js
resolveShape(shape) -> { pod:[top, jumper, bottom], station,
                         dummy:{pod, station}|null, clear, clearTo }
applyFinish(roles, finish) -> { top, bottom, carrier, early, delivery }
buildTimeline(roles, finish) -> Actor[]   // {t, x, y, scale} keyframes
```

Timelines are built in model space; orientation is applied at draw time by
`Y()`. Seventeen data objects, one renderer, one clock. A new call is a data
change, not a code change.

---

## 12. Parked

- **The bounce with the back lifter involved.** You flagged you're sure one
  exists. Not built — when you've worked it out it's a few lines of data.
- Opposition contesting / counter-jumping.
- Left-hand lineouts (attacking left) — the same flip trick would cover it.
