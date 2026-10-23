# Starter prompt: build a private AI day-coach on your Mac

Paste everything below the line into your AI agent (Claude Code or similar) on the Mac you want to coach. Fill in the three [BRACKETS] first.

---

You are going to help me build a private "day coach" on my Mac. It watches how I spend my screen time, writes me a short report every morning, proposes ONE time-saving fix a day, and checks later whether the fix worked. Work in phases. After each phase, show me proof it works before moving on. Ask me questions first (list in step 0), then build.

**My goal:** [e.g. spend most of my day in my dashboard, terminal and chat apps; cut context switching; protect 2 hours of focus]
**My Mac:** [model, RAM]
**Apps that are off-limits (never capture):** [password manager, banking, private browsing, anything else]

## Hard rules (never break these)
1. **Privacy.** Screenshots never leave this Mac, and never go to any cloud AI. Only local tools may look at images. Text derived from them (window titles, OCR text, one-line activity descriptions, counts) may be read by you. Check this with real evidence (network connections while it runs), not by assuming.
2. **Consent and safety.** This watches only my own screen. Skip the off-limits apps above, skip password managers and private windows automatically, give me a pause switch (a file such as `~/ScreenLog/PAUSED` stops everything), delete frames after 2 days, and show me exactly what is stored.
3. **Never delete or overwrite my files, and never send anything as me** (no messages, emails or posts) unless I explicitly say so for that action. Back up any file before you change it.
4. **Say "unknown" when you don't know.** A health check that reads "fine" when nothing is being measured is worse than none. Distinguish empty, failed and not-checked.
5. **The coach only coaches.** It writes the report and tracks results. It never builds things itself. Builds go to a separate working session after I say "build it".

## Step 0: ask me (max 6 questions)
My goal, off-limits apps, the time I want the report, where I want to read it (a chat channel, email, a file), whether I use a second screen, and which apps count as "home" (the ones I want to spend my time in).

## Phase 1: the screen log (local only)
- Use Hammerspoon (or similar) to: take a screenshot about every 3 seconds only when something changed; log one line per event: timestamp, app, bundle id, window title, idle flag; sample the window under the mouse pointer (display, x, y, plus a 0-1 position inside the window) about every 3 seconds while I'm active; count keystrokes, clicks and scrolls per minute as numbers only (never which keys); treat 60 seconds without input as idle.
- macOS will need Screen Recording, Accessibility and Input Monitoring permissions. Tell me exactly what to click; I'll grant them.
- Note: a normal screen snapshot does NOT include the mouse cursor, so the pointer must be logged separately.
- Prune frames after 2 days. Keep the text logs.

## Phase 2: turn pictures into text, locally
- OCR every saved frame with Apple Vision (a small Swift command-line tool). Add a per-frame time limit, a lock so two runs never overlap, and a marker for a frame that hangs it. (A hung OCR run once silently stopped logging for a full day.)
- Once a minute while I'm active, send one frame to a LOCAL vision model (LM Studio or Ollama, on this Mac or another machine on my own network) and ask for one short line: what I'm doing (reading, writing, scrolling, watching) and what kind of page (report, dashboard, chat, email, terminal...). Tell the model not to write names, numbers, passwords or message text. Start with a small vision model (about 4B parameters); large ones can crash on images or push other models out of memory. Measure seconds per frame and RAM. If the local machine is unreachable, queue and catch up. There is no cloud fallback.
- Trust the key/click/scroll counts over the description for typing versus reading, because the description comes from a single frame.

## Phase 3: the daily digest
A script that runs overnight and builds a text digest of yesterday: active time, idle time, time by app, top 30 windows, app switches per hour, longest focus block, my "home screen share" (percent of active time in my home apps), and the biggest external pages I kept opening. Per-minute streams (pointer, counts, descriptions) beat top-30 lists, which undercount.

## Phase 4: the coach
A scheduled job at [time] reads the digest and posts a short plain-language report with these sections: where the day went, what cost me time (with times), a fixes scorecard (minutes on the problem before versus since each fix), **one** proposed fix for today (what you saw, what you'd build, estimated minutes a day saved), and three things to try today. Rules:
- Real numbers only. Never invent activity that isn't in the digest.
- Keep a ledger file (one JSON line per fix: proposed, approved, routed, built, plus the baseline minutes and daily results after).
- Keep a corrections file. Before every report the coach reads it and obeys every line. When I correct the coach, add one dated line.
- Never quote passwords, card numbers, keys or private messages.
- When I reply "build it", the coach marks it approved and hands the spec to my build session. It never builds anything itself.

## Phase 5: safety nets
A health check that runs a few times a day and alerts me only on problems (logger not running, OCR stale, local model unreachable, disk getting full). Include a way to roll back every change.

## Optional Phase 6: message triage and draft replies
Only if I ask. Read my chat apps (read-only), label each message needs-reply, FYI or ignore, and put an UNSENT draft in the message box of conversations that need me. I send. Rules: never press send, block send actions in code, skip any conversation where I already typed something, and score each draft against what I actually sent (same, lightly edited, rewritten, unused). Expect only about 20-40 percent of drafts to be usable at first; never promise 98 percent. Warn me that other people's messages will be processed by an AI, and let me decide.

## How to work
- Write down the ways each phase could fail BEFORE writing code, then test those failures. Prefer end-to-end tests with a saved result I can inspect.
- Use a git repo, a changelog entry for every change, and one folder per phase.
- Work in small steps. After each phase, show me: what you built, evidence it works, what it costs (time, RAM, money), and what is still unverified. Don't say "done" until you have checked it yourself.
- Keep it simple. Short answers, plain language, no jargon without a one-line explanation.

Start with step 0.
