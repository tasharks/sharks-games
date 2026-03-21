# 🦈 Tel Aviv Sharks — Game Hub

## Project Overview
A collection of retro-style arcade games for Tel Aviv Sharks baseball players.  
Players earn weekly scores. All games share a global leaderboard via Supabase.

**Live URL:** `https://YOUR-GITHUB-USERNAME.github.io/sharks-games/`  
**Supabase Project:** (add your project URL here)

---

## 📁 File Structure

```
sharks-games/
├── index.html            ← Game Hub (main page)
├── shark-batter.html     ← Game 1: Shark Batter (Breakout-style)
├── game-2.html           ← Game 2: (next game)
└── README.md             ← This file
```

---

## 🗄 Supabase Setup (one-time)

1. Go to https://supabase.com → create free account → New Project
2. Name it `sharks-games`, pick a region close to Israel
3. Once created, go to **Settings → API**
4. Copy **Project URL** and **anon/public key**
5. Paste into BOTH files:
   - `index.html` → lines near top: `SUPABASE_URL` and `SUPABASE_KEY`
   - `shark-batter.html` → same variables near top of `<script>`

### Create the scores table

Go to **SQL Editor** in Supabase and run:

```sql
create table scores (
  id uuid default gen_random_uuid() primary key,
  game_id text not null,
  player_name text not null,
  score integer not null,
  wave integer default 1,
  week_start date not null,
  created_at timestamptz default now()
);

-- Allow anyone to read scores
create policy "Read scores" on scores for select using (true);
-- Allow anyone to insert scores
create policy "Insert scores" on scores for insert with check (true);

alter table scores enable row level security;

-- Index for fast leaderboard queries
create index scores_game_week on scores(game_id, week_start, score desc);
create index scores_week on scores(week_start, score desc);
```

---

## 🎮 Games Registry

| # | Name | File | Status | Style |
|---|------|------|--------|-------|
| 1 | Shark Batter | shark-batter.html | ✅ Live | Breakout/Arkanoid |
| 2 | TBD | game-2.html | 🔜 Coming | TBD |

---

## 🐙 GitHub Pages Deployment

1. Push all files to your repo
2. Go to repo **Settings → Pages**
3. Source: **Deploy from branch → main → / (root)**
4. Site will be live at `https://YOUR-USERNAME.github.io/sharks-games/`

To update a game: edit the file → commit → push. GitHub Pages auto-deploys.

---

## 🛠 Build Log

| Date | What was built | Notes |
|------|---------------|-------|
| 2025 | Shark Batter | Breakout-style, pixel art, mobile-first, Supabase leaderboard |
| 2025 | Game Hub | index.html with global leaderboard, weekly + all-time + by-game tabs |

---

## 🤖 Handing Off to a New Claude Session

Paste this into any new Claude chat to resume:

> "I'm Nolan, coach for the Tel Aviv Sharks youth baseball program.
> We're building a hub of retro arcade games at: https://YOUR-GITHUB-USERNAME.github.io/sharks-games/
> The repo is: https://github.com/YOUR-USERNAME/sharks-games
> Supabase project: YOUR_SUPABASE_URL
> Current games: Shark Batter (breakout style).
> See README.md in the repo for full context.
> Today I want to: [DESCRIBE WHAT YOU NEED]"

---

## 📱 Mobile Notes

- All games target iOS Safari (iPhone + iPad)
- Touch controls use `touchmove` + `touchstart` with `passive:false`
- Viewport: `width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no`
- Test on iPhone before deploying

---

## 🔑 Credentials (fill these in, keep private)

| Service | Value |
|---------|-------|
| GitHub repo | https://github.com/YOUR-USERNAME/sharks-games |
| Supabase URL | YOUR_SUPABASE_URL |
| Supabase anon key | YOUR_SUPABASE_ANON_KEY |
| Live site | https://YOUR-USERNAME.github.io/sharks-games/ |
