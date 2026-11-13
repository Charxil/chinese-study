# Chinese Study Notebook — App version
Created by Aye Chan

An installable phone app (PWA) for learning Chinese from vocabulary photos:
汉字 · pinyin · English · မြန်မာ meaning · မြန်မာ sound · character breakdown · example sentences · flashcards · audio.

## What works for everyone (no Claude, no account needed)
- All starter lessons, flashcards, 汉字-only mode, hide/reveal, 🔊 audio (phone's Chinese voice)
- Delete words, edit words, delete lessons
- Lessons are saved on the phone (IndexedDB) and work offline
- ⚙️ Settings → Backup / Restore to move lessons to another phone

## What needs AI (reading NEW photos, spelling check, typed copy, examples)
Choose ONE in ⚙️ Settings:
- **App server** – you run the free Cloudflare Worker in `server/worker.js` with YOUR Anthropic API key.
  Put its URL in `DEFAULT_PROXY` inside `index.html` and every user can read photos with no setup.
  You pay for their usage (roughly a few US cents per photo).
- **Own API key** – a user pastes their own key from console.anthropic.com (kept only on their phone).
- **Off** – study only.

---

## Step 1 — Put the app online (GitHub Pages), like KoNaWin-Beat
1. On github.com create a new public repo, e.g. `Chinese-Study`.
2. **Add file → Upload files** and upload everything in the `app` folder:
   `index.html, manifest.json, sw.js, icon-192.png, icon-512.png, icon-maskable-512.png`
3. **Settings → Pages → Branch: main / root → Save**.
4. After 1–2 minutes the app is at `https://<your-username>.github.io/Chinese-Study/`

## Step 2 — Install on a phone
- **Android (Chrome):** open the link → tap **⬇ Install app** (or ⋮ → Install app).
- **iPhone (Safari):** open the link → **Share → Add to Home Screen**.

## Step 3 (optional) — Turn on photo reading for all users
1. Get an API key: console.anthropic.com → API Keys → Create key. Add credit and
   **set a monthly spend limit** (Settings → Limits) so costs can't run away.
2. Create a free account at dash.cloudflare.com → **Workers & Pages → Create → Worker** → name it `chinese-study-ai` → Deploy.
3. **Edit code** → delete everything → paste `server/worker.js` → **Deploy**.
4. Worker → **Settings → Variables and Secrets**:
   - `ANTHROPIC_API_KEY` = your key (type **Secret**)
   - `ALLOWED_ORIGIN` = `https://<your-username>.github.io` (no slash at the end)
5. Copy the worker URL (e.g. `https://chinese-study-ai.<name>.workers.dev`).
6. In `index.html` find `const DEFAULT_PROXY = '';` and put the URL inside the quotes.
   Upload the new `index.html` to GitHub, and in `sw.js` change `cs-v1` to `cs-v2` and upload it too
   (this makes phones pick up the update).
7. Open the app → ⚙️ Settings → **Test connection** → should say *Connected ✓*.

## Updating the app later
Upload the new files to the same repo and bump `VERSION` in `sw.js` (cs-v2, cs-v3 …).
Users' lessons stay on their phones — updates never erase them.

## Later: Play Store / App Store
This same folder can be wrapped with **Capacitor** (or Bubblewrap/TWA for Play Store) without rewriting.
