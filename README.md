# Tab Out

**Keep tabs on your tabs.**

Tab Out is a Chrome extension that opens a toolbar dashboard of everything you have open. Tabs are grouped by domain, with homepages (Gmail, X, LinkedIn, etc.) pulled into their own group. Close tabs with a satisfying swoosh + confetti.

No server. No account. No external API calls. Just a Chrome extension.

This fork changes the launch behavior: Tab Out no longer replaces the browser's new tab page. It opens only when you click the Tab Out toolbar icon.

---

## Install with a coding agent

Send your coding agent (Claude Code, Codex, etc.) this repo and say **"install this"**:

```
https://github.com/Nita121388/tab-out
```

The agent will walk you through it. Takes about 1 minute.

---

## Features

- **See all your tabs at a glance** on a clean toolbar-launched grid, grouped by domain
- **Homepages group** pulls Gmail inbox, X home, YouTube, LinkedIn, GitHub homepages into one card
- **Close tabs with style** with swoosh sound + confetti burst
- **Duplicate detection** flags when you have the same page open twice, with one-click cleanup
- **Click any tab to jump to it** across windows, no new tab opened
- **Save for later** bookmark tabs to a checklist before closing them
- **Localhost grouping** shows port numbers next to each tab so you can tell your vibe coding projects apart
- **Expandable groups** show the first 8 tabs with a clickable "+N more"
- **100% local** your data never leaves your machine
- **Pure Chrome extension** no server, no Node.js, no npm, no setup beyond loading the extension

---

## Manual Setup

**1. Clone the repo**

```bash
git clone https://github.com/Nita121388/tab-out.git
```

**2. Load the Chrome extension**

1. Open Chrome and go to `chrome://extensions`
2. Enable **Developer mode** (top-right toggle)
3. Click **Load unpacked**
4. Navigate to the `extension/` folder inside the cloned repo and select it

**3. Click the Tab Out toolbar icon**

You'll see Tab Out.

If you previously loaded another copy of Tab Out from a different local folder, remove that copy from `chrome://extensions` first. Chrome remembers the local folder used for an unpacked extension, so keeping one stable development folder avoids confusion.

---

## How it works

```
You click the Tab Out toolbar icon
  -> Tab Out shows your open tabs grouped by domain
  -> Homepages (Gmail, X, etc.) get their own group at the top
  -> Click any tab title to jump to it
  -> Close groups you're done with (swoosh + confetti)
  -> Save tabs for later before closing them
```

Everything runs inside the Chrome extension. No external server, no API calls, no data sent anywhere. Saved tabs are stored in `chrome.storage.local`.

---

## Development notes

- Main extension folder: `extension/`
- Dashboard entry page: `extension/index.html`
- Toolbar click handler: `extension/background.js`
- Manifest file: `extension/manifest.json`
- Launch behavior: `chrome.action.onClicked` opens `index.html`
- New tab override: intentionally not used in this fork

After changing files, open `chrome://extensions` and click **Reload** on the Tab Out card.

For local development on this machine, the unpacked extension path is:

```text
E:\projects\tab-out\extension
```

Chrome displays extension pages as `chrome-extension://<extension-id>/index.html`. That is expected; it is the sandboxed extension URL, not a normal `file:///` URL.

---

## Tech stack

| What | How |
|------|-----|
| Extension | Chrome Manifest V3 |
| Storage | chrome.storage.local |
| Sound | Web Audio API (synthesized, no files) |
| Animations | CSS transitions + JS confetti particles |

---

## License

MIT

---

Built by [Zara](https://x.com/zarazhangrui)
