# TOEIC Dojo (ひかるのTOEIC道場)

A small web app I use every day to study for the TOEIC L&R test.

## Why I built it

I'm a 2nd-year commerce student at Fukuoka University, and I want to work abroad as a software engineer.
My TOEIC score is around 350, and my goal is 600 at the university TOEIC on December 19, 2026.

I learn best when I repeat things quickly many times and when studying feels like a game.
Generic study apps didn't fit that, so I made my own: short Part 5 drills, XP and levels, a streak counter,
and a review notebook for questions I get wrong.

## Features

- **Score tracker** – current score, target (600) and minimum line (500), plus a countdown to the test day.
- **Part 5 drill** – 5 grammar/vocabulary questions per round with a 30-second timer each.
  The difficulty adjusts to my accuracy, and wrong answers go to the review notebook automatically.
- **Vocabulary cards** – 10 words per round, 3 laps, with text-to-speech pronunciation (Web Speech API).
- **Review notebook** – a question "graduates" after I answer it correctly twice in a row.
  I can paste a list of mistakes from my textbook (出る1000) in a fixed format and import them.
- **Study time log** – a timer and manual entries per material, with a 7-day bar chart (SVG) and a monthly calendar.
- **Gamification** – XP, levels and a daily streak.

## How it works

Everything is in one file, `index.html` (HTML + CSS + plain JavaScript, no framework, no build step).

- **Rendering:** a single `render()` function rebuilds the page from the current state as an HTML string,
  then `bind()` attaches the click handlers again.
- **State:** two pieces of data:
  - `progress` – score, XP, streak, per-day study records, word status, per-question stats.
  - `reviews` – the questions in the review notebook.
- **Saving data:** the app has two modes.
  - *Local mode:* data is saved in the browser with `localStorage`. This works anywhere, including when you open `index.html` directly.
  - *Cloud mode:* when the page runs as a Claude artifact on claude.ai, it calls `window.claude.use('db')`
    and stores `progress` in the document `dojo/progress` and each review question in the collection `review`.
    Because the data lives there, my AI tutor (Claude) can add questions to my review notebook directly.

### Parts that only work inside Claude

`window.claude.use('db')` is provided by claude.ai artifacts, not by normal browsers.
Outside Claude, the app falls back to local mode automatically, so everything works,
but the data stays in that one browser and is not shared with my AI tutor.

## Run it

Open `index.html` in a browser. That's it.

## About the AI tutor ("Max")

I also study with an AI tutor I call Max. Max is not part of this repository:
it is Claude running in a claude.ai project with my own instructions (how to explain grammar, how to quiz me)
and a memory of my goals. This app is the practice tool; Max is the coach.

## Credits

Built with help from Claude (Anthropic). I'm studying the code and changing it step by step;
my own changes are recorded in the commit history.
