# D.R.I.P.

### Digitally Retained. Intelligently Processed.

**The stuff you save is intent you never get back to. D.R.I.P. turns it into a library you actually own — and, over time, into something you could publish.**

You save a workout. A recipe. A podcast with a strategy you meant to try. A how-to you wanted to come back to. Then the feed moves, life happens, and it's gone. Months later you save the same thing again.

It doesn't take thousands of saves for this to hurt. A hundred is already a pile you'll never sort by hand.

D.R.I.P. runs on your own machine, every morning, and does the sorting for you — turning each saved item into a clean, structured document, filed by topic. Keep going and each topic quietly compounds: your saved training clips become a training library; your saved "how to grow on Instagram" videos become a playbook; a year of saves becomes a guide you can rewrite and sell.

---

## The one promise that matters

**D.R.I.P. structures your content. It never invents it.**

Every figure, quantity, quote, and step in your documents is checked against the original source. If it wasn't in what you saved, it isn't in your document — no fake summaries, no invented steps, no made-up numbers, no hallucinated quotes. The only thing it adds is unit conversions of values you already saved (35 kg → 35 kg / 77 lb).

Most "AI" tools quietly embellish. D.R.I.P. has a verification layer that strips anything it can't trace back to your source. Structure added. Nothing fabricated. That's the product.

---

## How it works, end to end

1. **Capture** — D.R.I.P. reads what you've saved across seven platforms: YouTube (Watch Later, Liked, playlists), Instagram (saved), Facebook (saved), X (bookmarks), TikTok (favourites + liked), LinkedIn (saved), and browser bookmarks.
2. **Choose what counts** — at setup you decide, per platform and per folder, what gets processed. Saved a *MotoGP* playlist or a *banking* bookmark you only keep for quick access? Tell D.R.I.P. to skip it. It also reviews your loose, un-foldered bookmarks so the things you keep handy for visiting never become documents.
3. **Structure** — each item becomes a document built for what it actually is: a recipe → ingredient table (metric + imperial) + numbered method; a workout → sets/reps/rest plan; a podcast → discussion summary with quotes and jump-back timestamps; finance → a clean briefing with figures preserved exactly.
4. **Compound** — items of the same kind accumulate by topic. Each month and quarter, D.R.I.P. offers (never forces) to compile a period's collection into a single long-form guide — cover, contents, chapters — with an editable copy you can rewrite and even sell.
5. **Improve** — move a document to a different folder, or rate it keep/trash, and the sorting gets sharper every week. It learns your taste; no model retraining, all on your machine.

---

## Why it's different

- **It runs on your machine, not ours.** Your data never leaves your computer. No cloud upload. No subscription. No lock-in. The comparable tools are cloud-based and charge monthly. This one doesn't.
- **It keeps your secrets out of your files.** Session tokens and auth links are stripped before anything is saved. Login pages, homepages, feeds, and empty shells are filtered out — only things you genuinely saved to read get archived.
- **It's faithful by design.** The grounding check is the core feature, not a footnote.
- **It builds toward something sellable.** The compile step turns your own curation into an asset, not just tidy files.

---

## Your AI. Your cost. Your call.

Run it fully local and free with **Ollama** or **LM Studio**, or plug in your own **Claude / GPT / Grok** API key. Local or cloud, the same faithfulness rules apply. No API key required to get started.

---

## What's included

- Full pipeline: scraping, caption/transcript extraction, structured source-faithful documents
- 7 platform readers (YouTube, Instagram, Facebook, X, TikTok, LinkedIn, Bookmarks)
- A structured PDF **and** Markdown for every item, themed to the content
- Themed templates: recipes, workouts, podcasts, business, finance, tech, motivation, travel, health
- Source-grounding verification — anything not traceable to your source is removed
- Metric/imperial conversions carried on every measurement
- Timestamps on long videos and podcasts so you can jump back to the moment
- Per-platform, per-folder, and per-bookmark control over what gets processed
- New-folder detection — D.R.I.P. asks include/skip whenever a new folder appears
- Monthly & quarterly compile-to-sell — turn a period's saves into a long-form guide
- Self-learning sorting that improves from your folder moves and ratings
- One-command installer for macOS, Windows, and Linux
- Automated morning scheduler (launchd / systemd / Task Scheduler)
- Full documentation: HOW-TO-USE, FAQ, AI-SETUP

---

## Requirements

- macOS 10.15+, Windows 10+, or Linux
- Python 3.10+
- Ollama (free) **or** a cloud AI API key (Claude, GPT, or Grok)

---

## Quick start

```bash
# 1. Install (creates the environment and installs everything)
./install.sh            # macOS / Linux
#   install.ps1         # Windows (PowerShell)

# 2. Connect your accounts in the browser
python setup_cookies.py

# 3. Tell D.R.I.P. what to process, one platform and folder at a time
python onboarding.py

# 4. First run
./run_drip.sh
```

From then on it wakes every morning, processes your saves, and builds your library while you work.

---

## A note on how D.R.I.P. reads video

It never builds a library of downloaded video. It reads subtitles and captions only. For the minority of videos without captions, it downloads audio to a temporary file, transcribes it locally, and deletes it the moment transcription finishes. No video file is ever left on your drive.

---

## Support

Full documentation ships with the app. Questions: **info@thebvl.com**

---

*Built for people who save things they mean to come back to — but never do. Until now.*
