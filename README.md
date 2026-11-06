# Style Sprint: Catwalk Choices

An **original** HTML5 choice-runner game. Mechanics only inspired by the
generic "pick options at gates, judged at the end" idea — no copied art,
code, names, characters, or assets from Build a Queen or any existing game.
Every title, character, theme, outfit design, and piece of music is original
and drawn / synthesized procedurally.

## How to play

1. Your model sprints down the track. At each of **4 gates** per level, tap
   one of two outfit pieces — **Hat, Top, Bottom, Shoes**.
2. Match the level's theme for **+10 points** (plus up to +2 for a fast pick);
   an off-theme piece scores only **+3**. Too slow on a gate scores **+0**.
3. On the **catwalk**, your assembled look is scored against a rival stylist.
   Beat the rival's score to clear the level (**+20 bonus**); lose and you
   lose one of your **3 lives** and retry the level. Lose all 3 lives and
   it's game over.
4. **20 levels** with different themes — Beach Bash, Winter Gala, Neon Night,
   Retro 80s, Royal Ball, Space Cadet, Pirate Cove, Disco Fever, Cherry
   Blossom, Midnight Masquerade, Skate Park, and a Starlight Grand Finale.
   Difficulty rises: the rival's target score climbs and the per-gate timer
   shrinks from 9s (level 1) to ~4s (level 20).
5. Best score is saved in `localStorage`. Win all 20 levels for the
   champion screen.

Controls: tap / click a card, or keys **1 / 2** (or ← / →) to choose,
**P** pause, **M** mute. On-screen pause and mute buttons are top-right.

## Features

- Single self-contained `index.html` — plain HTML5 + vanilla JS + Canvas,
  no engine, no external assets, no network needed.
- Portrait-friendly (480×800 canvas, scales to any screen), mobile touch +
  mouse.
- Start menu with how-to-play, pause, mute, win screen, game-over / retry.
- Synthesized WebAudio SFX (pick good/bad, win/lose fanfares) and an original
  looping synth melody + bass line — no copyrighted music.
- All art is procedurally drawn on canvas (model, hats, tops, bottoms,
  shoes, runway, confetti). No stock images, no licensed characters.

## How to run

Open `index.html` in any modern browser (double-click, or serve it, e.g.
`python3 -m http.server` in this folder and visit `http://localhost:8000/`).
No build step.

## Headless test results (actually run, Nov 6 2026)

Tested with Playwright + headless Chromium (`/opt/meta-chromium/chrome`,
480×800 viewport) driving **real mouse clicks** on the canvas, polling
`window.__game` state. Full log from the final run:

- Load: menu state, level 1, score 0, lives 3, 20 themes — OK
- HOW TO PLAY click → howto screen; GOT IT → back to menu — OK
- START GAME click → gate state, level 1 — OK
- Pause button → `paused: true`; mute button → `muted: true` — OK
- Level 1, 4 gates, real clicks on the **good** card each time:
  player 47 (11–12 pts per pick) vs rival 18 → **LEVEL CLEAR**, +20 bonus,
  total 67, advanced to level 2 — OK
- Level 2, 4 gates, real clicks on the **bad** card each time:
  player 12 vs rival 23 → loss, lives 3 → 2, RETRY LEVEL returned to
  level 2, gate 1 — OK
- **Console / page errors: none.**
- Screenshots visually verified: menu, gate screen (timer, progress bar,
  two option cards), catwalk (rival vs player, score tally), level-clear
  screen.

Two issues found and fixed during testing (both verified by re-test):
1. Clicks were hit-tested against the previous frame's button list, so a
   slow frame could make a tap land on a stale button. Fixed: hit-testing
   is now computed from the current game state at click time.
2. The frame-time clamp (0.05s) made game time crawl under throttled
   headless rendering (measured 0.2–0.4 fps on a heavily loaded VM —
   even a blank page measured 0.2 fps, so this is environmental, not the
   game). Widened to a 0.5s clamp so game time tracks real time on slow
   devices. Also fixed two visual nits spotted in screenshots: menu
   tagline overlapping the model's feet, and lives hearts overlapping the
   pause/mute buttons.

Not tested in this pass: playing all 20 levels through to the champion
screen in one headless run (level 1→2 advance, win/loss/retry paths, and
the win screen code path are all exercised; the win screen triggers on
clearing level 20). Audio output was not auditioned (headless run is
muted); the WebAudio code paths ran with no errors.

## Files

- `index.html` — the whole game (28.5 KB)
- `README.md` — this file

Nothing was submitted to any portal. Build only.
