# D.R.I.P. — Frequently Asked Questions

---

## AI & Privacy

**Q: Does my saved content leave my Mac?**

It depends entirely on which AI provider you configure:

- **Local AI (Ollama, LM Studio):** Nothing leaves your Mac. 100% private.
- **Cloud AI (Claude API, OpenAI):** Summaries of your saved content are sent to the provider's servers for analysis. Their privacy policies apply.

D.R.I.P. defaults to `auto` mode, which tries local AI first and only uses cloud if a local option is unavailable AND you have a cloud API key configured. If you have not set any API keys, cloud AI is never used.

**Q: Can I use a different local AI model?**

Yes — D.R.I.P. is not tied to any specific model. Any model that runs in Ollama or LM Studio works. Change the model name in `config.json`. Examples:

```
llama3.2:3b      mistral:7b       qwen2.5:7b
deepseek-r1:8b   llama3.1:8b      phi3.5:3.8b
```

Pull a model with `ollama pull <modelname>`, then update `config.json`.

**Q: Can I use Grok / xAI?**

Yes — Grok is a first-class supported provider in D.R.I.P., not an afterthought. If you are on X or launching to an X audience, it is the natural choice. Set `provider` to `grok` in `config.json`, get an API key from [console.x.ai](https://console.x.ai), and you're done. Full setup in [`AI-SETUP.md`](AI-SETUP.md) under Option C.

**Q: Can I use a cloud AI I already pay for?**

Yes. D.R.I.P. natively supports Grok (xAI), Claude API (Anthropic), and OpenAI. For any OpenAI-compatible endpoint — including Groq, Together AI, Mistral Cloud, or a self-hosted model — change `openai_base_url` in `config.json` to that provider's endpoint and it works without any code changes. See [`AI-SETUP.md`](AI-SETUP.md) for full instructions.

**Q: What does it cost to run D.R.I.P. with cloud AI?**

Roughly per run:
- Grok 3 Mini: ~$0.01–$0.08
- Claude Sonnet: ~$0.02–$0.15
- GPT-4o-mini: ~$0.01–$0.10

With a local model the cost is zero. See [`AI-SETUP.md`](AI-SETUP.md) for a full breakdown.

---

## Video & Content

**Q: Does it download full videos?**

No. D.R.I.P. uses a subtitles-first approach: it downloads only the caption/subtitle file (a few kilobytes). If no subtitles exist, it downloads audio-only and runs Whisper transcription locally. Full video files are never downloaded.

**Q: What if a video has no subtitles?**

It falls back to Whisper AI, which transcribes audio locally on your Mac. This is slower but still private. Set `whisper_fallback: false` in `config.json` to skip this step if you prefer speed over coverage.

---

## Running & Automation

**Q: Does it run automatically?**

Yes. After running `setup_background.sh`, it runs every morning at 8 AM via macOS launchd. Your Mac needs to be on and awake at that time. If it's asleep, it runs the next time the schedule triggers while awake.

**Q: Can I run it manually?**

Yes. Run `bash run_drip.sh` from Terminal, or trigger it with `launchctl start com.drip.agent`.

**Q: What happens on the first run?**

It processes all your existing saved items (up to `max_items_per_platform` per platform, set in `config.json`). After that, every run only processes items saved since the last run.

**Q: How do I force it to re-process everything?**

Reset the state file:
```bash
echo '{"seen": {}, "last_run": null}' > SavedData/seen_items.json
```

The next run treats all saved items as new.

---

## Platform Scraping

**Q: Chrome is already open — will that cause problems?**

For YouTube, no — yt-dlp reads the cookie store directly regardless of whether Chrome is open.

For X, Instagram, Facebook, and TikTok — D.R.I.P. launches a separate headless Chromium browser (not your Chrome) and injects your Chrome cookies into it. Chrome can be open and running normally at the same time.

**Q: A scraper failed for one platform. What do I do?**

D.R.I.P. fails gracefully — one failed platform does not stop the others. Check the log (`Logs/drip-YYYY-MM-DD.log`) for the specific error. Common fixes:

- **"Not logged in"** — open Chrome, sign back into that platform, re-run `setup_cookies.py`, then re-run D.R.I.P.
- **"Private" or "403"** — your session cookie has expired. Re-run `setup_cookies.py`.
- **Timeout error** — try running with Chrome open in the background
- **Persistent failure** — disable that platform in `config.json` (`"enabled": false`) temporarily

**Q: Can I add custom YouTube playlists?**

Yes. Add any playlist URL to `config.json`:
```json
"custom_playlists": [
  "https://www.youtube.com/playlist?list=PLxxxxxxxxxxxxxx"
]
```

---

## TikTok

**Q: What is the difference between TikTok Saved and TikTok Liked?**

They are fundamentally different signals:

- **Saved (🔖 Favourites)** — you deliberately bookmarked a video to return to it. This is intentional. D.R.I.P. treats this as the primary source.
- **Liked (❤️)** — you hearted a video, often in the moment while scrolling. This is frequently a reflex, not a commitment to act. D.R.I.P. treats this as secondary.

D.R.I.P. always processes saved content first, up to your item limit. Liked content fills the remaining slots. If you never use the save button and only like videos, liked content is still fully collected — nothing is lost.

**Q: I only like videos — I never use the save button. Will D.R.I.P. work?**

Yes. If `include_liked` is true (the default), D.R.I.P. collects all your liked videos. You do not need to use the save feature. The distinction only matters for ordering: saved items are considered higher-intent and processed first when both sources are active.

**Q: I only post content — I don't like or save anything. Will D.R.I.P. work?**

Yes, but you need to enable own posts in `config.json`:

```json
"tiktok": {
  "username": "yourhandle",
  "include_saved":     false,
  "include_liked":     false,
  "include_own_posts": true
}
```

This tells D.R.I.P. to process your uploaded videos instead. Useful for creators who want to analyse and build on their own published content.

**Q: TikTok says "requires login" or "private" — what do I do?**

This means your session cookie has expired or was never captured. Fix:

1. Open Chrome and make sure you are logged into TikTok
2. Run `python setup_cookies.py`
3. Re-run D.R.I.P.

If the error is specifically about **liked videos** being private: go to TikTok → Profile → Privacy → Liked Videos → set to *Everyone*. This makes liked content publicly accessible and removes the cookie requirement for that source. Saved/Favourites always require cookies regardless of this setting — that is by TikTok's design.

**Q: I can't change my TikTok liked videos to public — it's a work account.**

Leave the setting private. As long as `setup_cookies.py` has been run with a valid session, D.R.I.P. can still read your private liked list. The public/private toggle only matters if you want access without cookies.

**Q: Does D.R.I.P. download TikTok videos?**

No. It reads playlist metadata only — video title, URL, uploader, and tags. No video files are downloaded or stored. If a description is available in the metadata it is also captured. Full video downloads never happen.

---

## X (Twitter)

**Q: Why does D.R.I.P. read bookmarks and not likes on X?**

On X, likes are a public social signal — they appear on your profile and are visible to others. They are not a reading list. Bookmarks are private, deliberate saves. They represent what you actually intend to return to. D.R.I.P. collects bookmarks only.

**Q: My X bookmarks aren't being collected.**

X Bookmarks require a valid session cookie. Re-run `setup_cookies.py` while logged into X in Chrome.

---

## LinkedIn

**Q: What does D.R.I.P. collect from LinkedIn?**

Saved posts — anything you bookmarked via the save icon on a post. LinkedIn saved posts are always private; no public setting exists. Your session cookie is required.

If you also publish your own posts and want D.R.I.P. to analyse them, enable own posts in `config.json`:

```json
"linkedin": {
  "username":          "your-linkedin-handle",
  "include_saved":     true,
  "include_own_posts": true
}
```

Your handle is the slug in your profile URL: `linkedin.com/in/YOUR-HANDLE`.

**Q: LinkedIn scraping fails immediately with "session expired".**

LinkedIn session cookies (`li_at`) can expire in as little as a week, especially if you log in from a new device or location. Re-run `setup_cookies.py` while logged into LinkedIn in Chrome. If it keeps expiring, make sure you are not using Chrome in private/incognito mode — that session is not saved to disk and cannot be read.

**Q: LinkedIn scraping fails with "access denied (403)".**

This usually means the CSRF token (`JSESSIONID` cookie) is missing or stale. Re-run `setup_cookies.py`. LinkedIn requires both `li_at` and `JSESSIONID` for API access — if either is missing, every request is rejected.

**Q: Why does LinkedIn show zero items even though my saved list has posts?**

LinkedIn's internal API occasionally changes its response structure. If no items are returned but no error is shown, check the log file for the raw status code. The scraper tries two API endpoints automatically — if both return empty, re-run `setup_cookies.py` and try again. If the problem persists across multiple fresh cookie runs, note it in the D.R.I.P. GitHub issues.

---

## Instagram & Facebook

**Q: Instagram saved posts aren't being collected.**

Instagram saved posts are always private. Your session cookie is required. If your account uses two-factor authentication, complete the login flow in Chrome before running `setup_cookies.py` — the cookie is captured after you are fully authenticated.

**Q: Facebook keeps failing after a day or two.**

Facebook sessions expire more aggressively than other platforms. If you notice recurring failures, re-run `setup_cookies.py`. You can also lower `max_items_per_platform` in `config.json` to reduce the session load per run.

---

## Troubleshooting

**Q: The LLM step failed. Will D.R.I.P. still produce output?**

Yes. If the LLM is unavailable or returns an error, D.R.I.P. uses a template-based fallback that generates basic outputs from the raw scraped content. The output is less personalised but the files are still created.

**Q: Ollama is installed but D.R.I.P. says it's unavailable.**

Make sure Ollama is running:
```bash
ollama serve
```

Or check that it's running as a background service:
```bash
pgrep -x ollama
```

If the process isn't running, start it or enable it to launch at login from the Ollama menu bar app.

**Q: How do I stop the background service?**

```bash
launchctl unload ~/Library/LaunchAgents/com.drip.agent.plist
```

To re-enable: `launchctl load ~/Library/LaunchAgents/com.drip.agent.plist`

**Q: How does the memory system get smarter over time?**

Two ways:
1. **Manual** — edit `Memory/user-preferences.txt` with plain English instructions any time
2. **Automatic** — `Memory/pattern-history.json` tracks which topics and categories appear most across your saves; the AI reads this on every run and adjusts its output focus accordingly

The longer you use D.R.I.P., the more its outputs reflect what you actually care about.
