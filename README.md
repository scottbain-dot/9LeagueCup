# League Cup

A courtside leaderboard & fixtures dashboard for a 5-lesson, 5-sport school competition.
Two leagues run in parallel (**Performance**, **Recreational**); each has five fixed
teams — **Red, Yellow, Green, Blue, Maroon** — that stay together all week. Each lesson is
one sport. Match points (win 3 / draw 1 / loss 0) accumulate across all five lessons; the
team with the most at the end wins its league.

The dashboard is **display-only**: all scoring maths lives in a Google Sheet, and the page
just reads and shows it. Mobile-first, built to be read on a phone next to the court.

## What it shows

- **Leaderboard** — teams sorted by points, big rank, team colour as the dominant visual.
  Reads the published **Standings** CSV from the Google Sheet (one per league) with a
  refresh button.
- **Today's fixtures** — pick a lesson (1–5) to see that lesson's sport, location
  (inside/outside), the four games across two rounds, and the referee team. Computed
  in-page from the fixed fixture pattern — no sheet needed.
- **League toggle** — switch between Performance and Recreational.

It's a single static `index.html` file — no build step, no backend, no browser storage.

## Setup

### 1. Host it
Upload `index.html` to this repo, then enable GitHub Pages:
**Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)` → Save.**
It goes live at `https://<your-user>.github.io/league-cup/`.

### 2. Connect the standings
Fixtures work immediately. For the leaderboard, publish each Standings tab from the
Google Sheet — **File → Share → Publish to web → select the Standings tab → CSV** — then
paste the two links into the config block near the top of `index.html`:

```js
const STANDINGS_CSV = {
  Performance:  'https://docs.google.com/.../pub?gid=...&single=true&output=csv',
  Recreational: 'https://docs.google.com/.../pub?gid=...&single=true&output=csv'
};
```

Until the links are added, the leaderboard shows a friendly "not connected yet" message.
After an edit in the sheet, the published CSV refreshes within a minute or two.

## Sheet structure (source of truth)

Per league, a **Results** tab holds the 20 fixtures with two score columns teachers type
into, plus helper columns that convert each game to points. A **Standings** tab sums those
into Played / Points / Rank per team. The dashboard reads only the Standings tab — it does
no scoring of its own, so there's no logic to keep in sync between the sheet and the page.

## Sports per lesson

| Lesson | Performance | Recreational |
|--------|-------------|--------------|
| 1 | Soccer (out) | Benchball (in) |
| 2 | Basketball (in) | Handball (out) |
| 3 | Volleyball (in) | Kickball (out) |
| 4 | Capture the Flag (out) | Volleyball (in) |
| 5 | Benchball (in) | Capture the Flag (out) |

Over the five lessons the fixture pattern forms a full double round-robin: every team plays
all four opponents twice (8 games) and referees exactly twice.

To change sports, team colours, or fixtures, edit the `SPORTS`, `TEAM_COLOR`, and
`FIXTURES` constants at the top of `index.html`.
