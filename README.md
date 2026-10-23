# coach

A starter prompt for building a private AI "day coach" on your own Mac.

It watches how you spend your screen time, writes you a short report every morning, proposes **one** time-saving fix a day, and checks later whether the fix worked. Screenshots stay on your machine and are only ever read by local tools.

## Use it

1. Open [`PROMPT.md`](PROMPT.md).
2. Fill in the three `[BRACKETS]` at the top: your goal, your Mac, and the apps that must never be captured.
3. Paste everything below the line into your AI agent (Claude Code or similar) on the Mac you want to coach.
4. Let it ask its questions, then build phase by phase. Make it show you proof at the end of each phase.

## What you get

1. **A local screen log:** screenshots on change, window titles, mouse pointer position, and key/click/scroll counts (numbers only, never which keys).
2. **Pictures turned into text, locally:** OCR plus a one-line description per minute from a local vision model. No cloud model ever sees a screenshot.
3. **A daily digest** of where your time went.
4. **A daily coach report** with one proposed fix, and a ledger that measures whether each fix worked.
5. **Health checks** that say "unknown" instead of "fine" when nothing is being measured.
6. *(Optional)* **Message triage** that leaves unsent draft replies for you to edit and send yourself.

## Read this first

- **It is untested on anyone else's machine.** It was written from one working setup, so expect setup problems.
- **It assumes a Mac** (Hammerspoon for capture, Apple Vision for OCR). On Windows or Linux the capture phases need different tools.
- **It watches your screen.** Only run it on a computer you own, keep the off-limits list honest, and use the pause switch. macOS will ask for Screen Recording, Accessibility and Input Monitoring permissions.
- **Message drafting (phase 6) means an AI reads other people's messages.** Check your employer's policy and decide for yourself before turning it on.
- **Expect modest draft quality.** In the original setup roughly 20 to 40 percent of drafts were usable as-is or after a light edit. Don't expect 98 percent.
