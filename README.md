# Ledger

A private daily log. Mark the day, write what you did, keep the run alive.

Everything lives in `index.html` — one file, no build step, no server, no account.
The other files only exist to make it installable and work offline.

```
index.html                 the whole app (including sync)
manifest.webmanifest       install metadata (name, icons, shortcuts)
sw.js                      offline cache
icon.svg / icon-*.png      app icons
```

---

## Getting it running

### Option 1 — Just open it (30 seconds, no installing anything)

Download `index.html`, double-click it. Done. It works offline and saves to that
browser. You won't get the dock icon or the home-screen icon, but everything else works.

Good for: trying it out today.

### Option 2 — Host it once, install it everywhere (10 minutes, recommended)

This is what gets you the pinned app on your laptop **and** the icon on your phone.
You need all the files in one folder, hosted at a URL.

**GitHub Pages** — free, permanent URL, easy to update later:

1. Make a new repository on github.com (public is fine — your entries are never in
   these files).
2. Upload all the files into it.
3. **Settings → Pages → Deploy from branch → main → /(root) → Save**.
4. Wait a minute. Your URL appears at the top of that page.

To update later: drag the new files onto the repo's file list to overwrite, commit,
wait a minute, then hard-refresh your site with `Ctrl Shift R`. You never need to
delete or recreate the repository.

**Netlify Drop** — no account needed to start, free forever:

1. Put all the files in one folder on your laptop.
2. Go to **app.netlify.com/drop**
3. Drag the whole folder onto the page.
4. You get a URL like `https://gentle-otter-4f2a91.netlify.app`. That's your app.
5. Sign up (free) if you want to keep the URL permanently, and rename it to
   something you'll remember under **Site settings → Change site name**.

Alternatives if you'd rather: **GitHub Pages** (free, needs a GitHub account —
make a repo, upload the files, Settings → Pages → deploy from `main`), or
**Vercel** / **Cloudflare Pages** (both free, both drag-and-drop).

It must be **https** — service workers and installation don't work over plain http.
All the options above give you https automatically.

---

## Pinning it

### On your laptop (Chrome, Edge, Brave, Arc)

Open your URL. Look in the address bar for an **install icon** (a monitor with a
down-arrow, on the right side). Click it → **Install**.

If you don't see it, use the **Install** button in Ledger's own header — it appears
automatically when your browser will accept it. Or: **⋮ menu → Cast, save and share →
Install page as app**.

You now have Ledger in your dock/taskbar as its own window, with no browser chrome.
Right-click the icon for the **Mark today done / Partial / Write note** shortcuts.

**Safari on Mac:** File → Add to Dock.

**Firefox:** doesn't support installing web apps. Pin the tab instead —
right-click the tab → Pin Tab. It'll survive restarts.

### On your phone

**Android (Chrome):** open the URL → **⋮** → **Add to Home screen** → **Install**.
You'll get a real app icon and a full-screen app.

**iPhone (Safari — must be Safari, not Chrome):** open the URL → the **Share**
button → scroll down → **Add to Home Screen**.

---

## Opening it on another laptop

Three different situations, so pick the row that matches you.

| What you want | What to do | Does your data come with you? |
|---|---|---|
| Just see your log on a borrowed machine | Open your Ledger URL in any browser | **No** — that machine starts empty |
| Set up your own second laptop properly | Open the URL → Install → then **Restore** a backup | **Yes**, via the backup file |
| Never think about backups | Use the version inside Claude | **Yes**, automatically |

### Moving your data to a second laptop, step by step

**On the laptop that has your data:**

1. Click **Backup** at the bottom of the app (or press `⌘K` / `Ctrl K` and type "backup").
2. Click **Download .json**. You get `ledger-2026-08-24.json`.
   (There's also **Export as CSV** in the palette if you want it in a spreadsheet.)
3. Put that file somewhere both machines can reach — Google Drive, email it to
   yourself, a USB stick, WhatsApp to yourself. Anything.

**On the new laptop:**

4. Open your Ledger URL and install it.
5. Click **Restore** at the bottom.
6. Drag the `.json` file into the box, or click **Choose file**.
7. Click **Restore**.

Restoring **merges** — it never wipes what's already there. If the same day exists in
both places, the more recently edited one wins. So you can restore an old backup
without losing this week's entries.

---

## Turning on sync (optional)

Everything above works without this. Sync is for when you want your laptop and
phone to hold the *same* ledger automatically, with no backup files.

You do it **inside the app** — there's nothing to edit on your computer, no files
to change, no re-uploading to Netlify. Open Ledger, click **Sync off** in the
bottom-right corner, and follow the five steps. It takes about five minutes once.

What the steps cover:

1. Make a free **Supabase** account and project (this is just the box your entries
   live in — you never have to understand it)
2. Paste one block of setup code the app gives you, press Run
3. Turn off email confirmation, so signing in is instant
4. Paste two values from Supabase into the app — it tests them and tells you
   immediately if something's wrong
5. Create an email + password login

Then on your **phone and any other laptop**: open the same web address, click
**Sync**, and choose **Sign in** with that same email and password. Your whole
history appears on its own.

After that it just runs. Type on your phone, it's on your laptop within seconds.

**Good to know:**

- The bottom-right corner always shows the truth — *Synced just now*, *Syncing…*,
  *Offline*, or the specific thing that went wrong.
- **Offline still works.** Write on a plane; it uploads when you reconnect.
- Edit the same day on two devices and the **most recent edit wins**.
- Deleting a day deletes it everywhere — deletions travel properly rather than
  reappearing on the next sync.
- Only your account can read your entries. That's enforced by the database, not
  just by the app.
- **Sign out** keeps your entries on that device. **Disconnect** unlinks sync
  entirely. Neither one deletes anything.
- Your theme and weekly target stay per-device on purpose — dark on the laptop and
  light on the phone is a reasonable thing to want.

Backup and restore keep working exactly as before, sync or no sync. Worth doing
occasionally either way.

---

### The thing to understand about where data lives

Ledger stores your entries **in the browser on the machine you're using**. It does not
have a server, which is why there's no signup and no one else can ever read your log —
but it's also why the data doesn't teleport between devices on its own.

The footer always tells you which mode you're in:

- **Saved on this device** — standalone, no sync set up. Backups move your data.
- **Saved on this device** + **Synced just now** — sync is on. Nothing to do; your
  devices keep themselves matched.
- **Synced to your account** — you're running it inside Claude, which syncs for you.

Two habits worth keeping:

- Download a backup once a month. Ledger will nudge you if it's been a while.
- Don't "Clear browsing data → cookies and site data" for your Ledger URL. That
  wipes it. Clearing cache alone is harmless.

---

## Using it

| Key | Does |
|---|---|
| `1` `2` `3` | Mark today done / partial / missed |
| `←` `→` | Move a day back or forward |
| `T` | Jump to today |
| `N` | Write a note |
| `/` | Search the log |
| `⌘K` / `Ctrl K` | Command palette — everything lives here |
| `⌘Z` / `Ctrl Z` | Undo (add `Shift` to redo) |
| `,` | Settings |
| `Esc` | Close anything, or clear a tag filter |

### Search operators

Type these in the search box or the command palette, alone or combined:

| Type | Finds |
|---|---|
| `gym` | notes containing that word |
| `#dsa` | days tagged `#dsa` |
| `#dsa #work` | days with **both** tags |
| `status:done` | also `partial`, `missed`, `none` |
| `has:note` | days you actually wrote something on |
| `before:2026-08` | anything earlier |
| `after:2026-01-01` | anything later |
| `status:done #gym after:2026-06` | all of it at once |

### Settings

Press `,` or click **Settings** at the bottom. All in one panel:

- Dark or light
- Year grid or month calendar
- Weeks starting Monday or Sunday
- Weekly target
- **Rename the three marks** — if you're tracking gym, "Full / Light / Rest" may suit
  you better than "Done / Partial / Missed"
- Daily reminder time, and which days it should nudge you

### Notes formatting

`**bold**` and `` `code` `` render in the log. Tags highlight automatically.

### Daily reminders

Turn on in Settings, pick a time and which days. It only nudges you if the day is
**still blank** — never if you've already logged.

Honest limitation: a website can only fire reminders while it's open or running in
the background. Installed on a laptop that's on, it's reliable. On a phone that's
fully closed, it may not fire — a phone alarm is still the sure thing. Doing better
needs a notification server, which is more setup than it's worth here.

### Milestones

Hit 7, 14, 30, 100, 365 days in a row — or 10, 50, 100, 500 days logged — and it
says so. Once each, never nagging.

### Tag detail

Click a tag in the Insights section for its own page: how many days, its longest
streak, what share of your log it covers, when you last touched it. This is how you
track several habits without the app becoming a spreadsheet.

**The command palette** (`⌘K`) is the fast way around. Type a word to search your
notes, a date to jump to it (`12 aug`, `2026-03-09`, `7 days ago`, `yesterday`), a
`#tag`, or the name of any action.

**Tags** — write `#gym` or `#dsa` in a note and it becomes a filter. Click any tag to
see only those days; the year grid dims everything else. Works in any language.

**Partial counts toward your streak.** Showing up badly still beats not showing up.
An unmarked *today* doesn't break your run either — the day isn't over yet.

**Weekly target** — click the ring in the header to set how many days a week you're
aiming for. Monday to Sunday.

---

## If something goes wrong

**Changes aren't saving.** Check the footer. If it says *"Not saving"*, you're in a
private/incognito window, or the browser is blocking storage for the site. Use a
normal window.

**The app won't install.** It has to be served over https, and all the files need to
be in the same folder. Opening `index.html` straight off your disk won't offer
installation — that's a browser rule, not a bug.

**I updated the files but see the old version.** The service worker caches
aggressively. Hard-refresh with `Ctrl Shift R` / `⌘ Shift R`, and bump the `CACHE`
value at the top of `sw.js` when you change anything.

**Fonts look plain.** Ledger loads two fonts from Google Fonts. Offline on first
load, or behind a network that blocks them, it falls back to your system fonts.
Everything still works.

**Sync says "Table missing".** The setup code in step 2 didn't run. Open Supabase
→ SQL Editor, paste it again, press Run.

**Sync says "Signed out — sign in again".** Sessions expire eventually. Click the
sync indicator and sign in again; nothing is lost.

**My phone shows nothing after signing in.** Give it a few seconds, then click the
sync indicator and hit **Sync now**. Also double-check you used the *same* email as
your laptop.

**I erased it by accident.** Restore your most recent backup. If you don't have one,
it's gone — there's no server holding a copy. This is the tradeoff for a log nobody
else can read.

---

## Your data

Plain JSON, yours, portable:

```json
{
  "v": 2,
  "entries": {
    "2026-08-24": { "s": "done", "n": "Shipped the auth flow. #dsa", "u": 1756000000000 }
  },
  "settings": { "theme": "dark", "target": 5 }
}
```

`s` is the status, `n` is your note, `u` is when you last edited it. Nothing is
obfuscated and nothing is uploaded anywhere.
