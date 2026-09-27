# 🀄 Ceki Score Tracker

A clean, dark-themed web app for tracking scores in **Ceki** — a traditional Indonesian card game. Built as a single HTML file with a Supabase backend, so everyone at the table can see scores in real time from any device.

![Ceki Score Tracker](https://img.shields.io/badge/Built%20with-Supabase-3ECF8E?style=flat&logo=supabase&logoColor=white)
![Deployed on](https://img.shields.io/badge/Deployed%20on-Vercel-000000?style=flat&logo=vercel&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat)

---

## ✨ Features

- 🎴 **Session management** — create named game sessions with 4 players
- 📊 **Live leaderboard** — real-time score totals with rank medals (🏆🥈🥉)
- 🖱️ **One-tap score entry** — tap a player card → popup → done
- ⏳ **Round tracking** — shows who has submitted for the current round and who hasn't
- 📋 **Round history** — full table of every completed round
- 🌐 **Multi-device** — share the URL and everyone sees the same data
- 🗑️ **Session deletion** — clean up old sessions anytime
- 🌑 **Dark mode** — always dark, easy on the eyes during night games

---

## 🛠️ Tech Stack

| Layer    | Technology |
|----------|------------|
| Frontend | Vanilla HTML + Tailwind CSS |
| Database | [Supabase](https://supabase.com) (PostgreSQL) |
| Hosting  | [Vercel](https://vercel.com) |

No build step. No framework. Just one HTML file.

---

## 🚀 Self-Hosting Guide

### 1. Clone the repo

```bash
git clone https://github.com/aditriyas/ceki-tracker.git
cd ceki-tracker
```

### 2. Set up Supabase

1. Create a free project at [supabase.com](https://supabase.com)
2. Go to **SQL Editor** and run:

```sql
CREATE TABLE sessions (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  name text NOT NULL,
  players text[] NOT NULL,
  created_at timestamptz DEFAULT now()
);

CREATE TABLE rounds (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  session_id uuid REFERENCES sessions(id) ON DELETE CASCADE,
  scores jsonb DEFAULT '{}',
  round_number integer NOT NULL,
  is_complete boolean DEFAULT false,
  created_at timestamptz DEFAULT now()
);

ALTER TABLE sessions ENABLE ROW LEVEL SECURITY;
ALTER TABLE rounds ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow all" ON sessions FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "Allow all" ON rounds FOR ALL USING (true) WITH CHECK (true);
```

3. Go to **Settings → API** and copy:
   - **Project URL** → `https://xxxx.supabase.co`
   - **anon public** key → `eyJ...`

### 3. Configure the app

Open `ceki_tracker.html` and replace the credentials at the bottom of the file:

```js
const db = createClient(
  'https://YOUR_PROJECT_ID.supabase.co',  // ← your Project URL
  'YOUR_ANON_KEY'                          // ← your anon public key
);
```

### 4. Deploy to Vercel

```bash
# Option A: Connect GitHub repo to Vercel (recommended)
# Go to vercel.com → New Project → Import from GitHub

# Option B: Vercel CLI
npm i -g vercel
vercel
```

---

## 🎮 How to Play

1. **Create a session** — enter session name + 4 player names → Start
2. **View the scoreboard** — click any session card from the home screen
3. **Enter scores** — tap a player's card after each round → type the score → Save
4. **Track progress** — the leaderboard updates instantly, round history is always visible

> Scores can be **positive** (win) or **negative** (loss). The app tracks cumulative totals across all rounds.

---

## 📁 Project Structure

```
ceki-tracker/
└── ceki_tracker.html   # Entire app — HTML + CSS + JS in one file
```

---

## 🤝 Contributing

Pull requests are welcome! Feel free to open an issue for bugs or feature requests.

---

## 📄 License

MIT — free to use, modify, and distribute.
