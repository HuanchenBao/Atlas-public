# Atlas 0.24

Atlas combines visual notebooks, independent idea Blueprints, local dictation and linked Codex conversations.

## Installation

Requires an Apple Silicon Mac (arm64) with macOS 14 or later. Download `Atlas-0.24-macOS-AppleSilicon.dmg`, open it, drag **Atlas** into **Applications**, then open Atlas from Applications. The app and installation image are Developer ID signed and notarized by Apple. Model downloads require an internet connection; weights are not included. The companion service includes its own Python runtime; you do not need Python or Xcode command-line tools for Atlas itself.

For dictation, allow Microphone access. For inserting text into other apps with the Fn shortcut, allow Atlas in **System Settings → Privacy & Security → Accessibility**. Atlas shows permission/setup guidance if access is missing. macOS permission decisions belong to you.

## What changed

- **Notebook-wide Blueprints:** Organize synthesizes concepts and relationships across all active notes, with staged synthesis for large notebooks. Idea cards have independent identities and may cite zero, one or several notes. Incidental source metadata such as checksums is kept out of the main concept map unless requested. Unchanged automatic outline clutter can be replaced; authored cards and Codex placements/collections stay protected. Use **Blueprint options → Organize notebook Blueprint with AI**. **Restore previous organization** recovers a saved map.
- **Codex collections:** Double-click opens the collection’s own 2D Blueprint. Blank canvas clicks stay in that Blueprint. The Info list closes when you click empty space outside it.
- **Card appearance:** Chat and collection color menus support saved custom colors and a Default reset. Their fills are more translucent than note cards; source and attention cues remain separate from color.
- **Execution visibility:** Running, attention and recent-finish indicators distinguish current evidence from last-known results and imported history. Manual Refresh status requests a source update. Optional reviewed Codex hooks provide future local desktop lifecycle events; notification preferences are available from **Codex activity and notifications**.

## Connect your own Codex source

1. Install the official Codex desktop app or Codex CLI and sign in to **your own** Codex account. Atlas uses the supported local Codex App Server/CLI; it does not include an account or a Codex login token. A working `codex` executable in your PATH, or the supported installed desktop CLI, is needed for live local history retrieval.
2. Open Atlas’s **Codex chats** sidebar, then **Codex account sources** to inspect account/source coverage. Local history and the CLI Cloud task list are separate sources. Refresh status checks configured sources; double-click a chat to open its original destination in Codex. Keep Codex signed in to the matching account.
3. For prompt local desktop execution events, choose **Codex account sources → Set up live desktop status… → Prepare bridge**, review the bridge and proposed hook definition, then **Install hooks for review…**, and explicitly review/trust them in Codex **Settings → Hooks**. They apply to future events. Atlas retains only identifiers, status and timestamps for the bridge. Enable Atlas notifications in macOS if you want system banners; in-app indicators remain available when notifications are off.
4. Desktop Cloud history and desktop remote-control pairing are not automatically shared with Atlas. **Import Codex desktop history…** accepts an Atlas history JSON export from the intended account; these are snapshots, not live execution feeds. Ordinary conversation links can be added using **Add Codex link**: copy the conversation’s `codex://threads/…` deep link, not a file link. Atlas does not generate a complete desktop-history export itself.
5. Continuous remote history requires a separately configured, already trusted SSH connection to a Mac with Codex App Server and the intended signed-in profile. Remote-control pairing alone is insufficient. Advanced sources are configured in `~/Library/Application Support/Conversation Atlas/atlas.local.json`; see the source coverage shown in Atlas. Imported or disconnected sources do not claim live status.

### Advanced SSH source example

After configuring and testing a trusted SSH alias such as `my-mac-studio` yourself, merge an entry into `~/Library/Application Support/Conversation Atlas/atlas.local.json` (preserve any existing sources):

```json
{
  "sources": [
    {"id":"local","kind":"local","label":"Local Codex"},
    {"id":"cloud","kind":"cloud","label":"Codex Cloud"},
    {"id":"home-studio","kind":"ssh","label":"My Mac Studio","host":"my-mac-studio"}
  ]
}
```

Quit and reopen Atlas to reload configuration. The remote host must already trust your SSH key, have its host key recorded locally, and expose `codex` in the noninteractive SSH PATH with the intended account signed in. Atlas uses batch-mode SSH and does not set up keys, accept unknown host keys, or reuse desktop remote-control pairing. Current execution visibility depends on the supported source/runtime; remote history alone does not guarantee live desktop turn status. Use a Codex version supporting `app-server --listen stdio://`; optional hook support must also be available in your installed Codex version.

## Calendar

Go to **Settings → Connections & Sync → Calendar → Connect Apple Calendar…** and grant **Full Access** when macOS asks. Atlas reads calendars from the accounts you have configured in the macOS Calendar app. Choose a default account/calendar if desired. If access was denied, use **Open Calendar Privacy Settings…** or **System Settings → Privacy & Security → Calendars**, enable Full Access for Atlas, then **Refresh status** / **Check Calendar read access**. If no calendars appear, add or enable your account in macOS Calendar. Atlas changes events only when you request it.

## Dropbox

**Dropbox connection for other users is pending.** The current provider registration is in Development / Only you. This release does not promise public Dropbox sign-in or sync. Local notebooks work independently. No Dropbox provider settings or approval status were changed for this release.

## Speech and formatting models

Open **Settings → Voice & Shortcuts** to choose and download speech models. The catalog retains Cohere Transcribe Q4, multilingual Whisper variants and Qwen3-ASR options; choose an appropriate language/model for dictation or meetings. Downloads are pinned and checked by size and SHA-256. Local recognition runs offline after download. Apple Speech is available on supported Macs running macOS 26 or later.

After-recording formatting starts Off. The public local formatting options are **AgenticASR-Refiner (4-bit)** and **Qwen3-4B-Instruct-2507 (4-bit)**, downloaded from their pinned Hugging Face repositories. No owner fine-tuned models, personal Spokenly V4 assets/catalog entries, model weights, notebooks, conversation history or credentials are bundled. Original recognition and formatter output remain separate for review. An optional GPT connection for Assistant/AI Blueprint requests is configured separately; those requests send the relevant notebook content to the selected connection.

Third-party licenses and notices are included in the app. Check `SHA256SUMS.txt` against the downloaded installer using `shasum -a 256`.
