# CleverSearch — Thunderbird Extension

An AI-powered email query and analysis tool for Thunderbird. Ask open questions about your emails and get ranked results with an optional AI-generated summary. Part of the **ThunderClass** project.

## Features

- **Natural-language queries** — ask any question and CleverSearch scores every email for relevance (0–100) with a one-line reason
- **Two search scopes** — query your currently selected emails, or pick folders + a date range
- **Configurable parse fields** — choose which email fields to send to the AI: subject, sender, recipients, body, and/or attachment presence. Fewer fields = larger batches = faster
- **Smart batching** — emails are grouped into token-aware batches to keep prompts bounded
- **Optional answer summary** — the AI generates a 3–5 sentence direct answer from the top-ranked emails
- **Port-based streaming** — uses a persistent Port connection (no 30-second timeout), with progress updates as each batch completes
- Works with **any OpenAI-compatible `/v1/chat/completions` endpoint** — cloud or self-hosted
- Settings link in the popup opens the full options page

## Install

1. Thunderbird → **Tools → Add-ons and Themes** → gear icon → **Install Add-on From File…** → select `cleversearch.xpi`.

## Configure

Open CleverSearch options (from the Add-ons Manager or the Settings link in the popup):

- **API base URL** — default `https://api.openai.com`; for your local server use e.g. `https://192.168.50.58:8443` (no trailing slash, no `/v1`).
- **API key** — leave blank for local endpoints that don't require one.
- **Model** — any model name your endpoint accepts (e.g. `gpt-4o-mini`, `google/gemma-4-26B-A4B-it`).
- **Max tokens per batch** — controls how many emails fit per API call (default 3000).
- **Temperature** — 0 = precise, 1 = creative (default 0.2).

Click **Test endpoint** to verify connectivity.

## Use

1. Select email(s) in Thunderbird, or switch to "Folders + dates" mode in the popup.
2. Choose which fields to parse (Subject, Sender, Body, etc.).
3. Type your question and click **Search**.
4. Review ranked results with scores and reasons.
5. If summary is enabled, read the AI's direct answer at the top.

## Backup & Restore

At the bottom of the options page:

- **Export config** — downloads a JSON file with all settings: API config, extra classification rules, account selection, and destination folder exclusions (stored as portable folder paths).
- **Import config** — reads a JSON file and restores all settings. Folder exclusions are matched by path, so they work across Thunderbird sessions and machines.

## Files

```
cleversearch/
├── manifest.json
├── background.js           # message handlers, AI calls, Port streaming, folder enumeration
├── popup/
│   ├── popup.html/js/css   # query UI with results
├── options/
│   ├── options.html/js/css # AI backend settings
├── icon-16.png
├── icon-32.png
└── icon-64.png
```

## Notes

- Built against Thunderbird 128+ (Manifest V3 MailExtension).
- Body content is truncated to ~6000 chars before being sent, to keep prompts bounded.
- The API key is stored in `messenger.storage.local` — not encrypted.
- Uses tolerant JSON parsing (same approach as ThunderClass) to handle models that wrap output in prose.
- Settings keys are prefixed with `cs_` to avoid conflicts when both ThunderClass and CleverSearch are installed.
