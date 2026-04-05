# Genealogy Research Assistant

An **AI Edge Gallery Agent Skill** that turns your on-device LLM into a genealogy research companion. Built for the [Google AI Edge Gallery](https://github.com/google-ai-edge/gallery) app, it runs entirely on your phone — no cloud, no account, complete privacy for your family data.

## What It Does

| Capability | Description |
|---|---|
| **Record Ancestors** | Capture names, dates, places, and notes in structured summary cards |
| **Family Group Sheets** | Build standard husband / wife / children group sheets |
| **Source Citations** | Format citations following *Evidence Explained* conventions for census, vital, church, immigration, military, and other record types |
| **Research Logs** | Document every search — what you looked for, where, and what you found |
| **GEDCOM Export** | Generate GEDCOM 5.5.1 data you can import into any genealogy software |
| **Research Guidance** | Get advice on record types, repositories, brick-wall strategies, paleography, and more — powered purely by the LLM |

## Getting Started

### Prerequisites

- [Google AI Edge Gallery](https://play.google.com/store/apps/details?id=com.google.ai.edge.gallery) app (Android 12+ or iOS 17+)
- A downloaded on-device model that supports Agent Skills (e.g. Gemma 4)

### Install the Skill

1. Open the AI Edge Gallery app.
2. Navigate to **Agent Skills → Skill Manager**.
3. Tap **Load from URL** and enter:
   ```
   https://jonahlyn.github.io/genealogy-research-assistant
   ```
   > **Note:** This skill requires [GitHub Pages](https://pages.github.com/) hosting because it includes JavaScript. Standard GitHub repo URLs (`github.com/...`) won't work — use the GitHub Pages URL above.
4. The skill appears in your skill list and activates automatically when you ask about family history.

### Set Up Google Drive Storage

This skill saves your data to Google Drive. You'll need to provide OAuth credentials as the skill secret. This is a one-time setup.

**Step 1 — Create a Google Cloud project and OAuth credentials**

1. Go to [console.cloud.google.com](https://console.cloud.google.com) and create a new project.
2. Go to **APIs & Services → Library**, search for **Google Drive API**, and enable it.
3. Go to **APIs & Services → OAuth consent screen**. Choose **External**, fill in an app name (e.g. "Genealogy Skill"), and add your Google account as a test user.
4. Go to **APIs & Services → Credentials → Create Credentials → OAuth client ID**.
   - Application type: **Desktop app**
   - Give it a name and click **Create**.
5. Copy the **Client ID** and **Client secret** shown.

**Step 2 — Get a refresh token**

1. Go to [developers.google.com/oauthplayground](https://developers.google.com/oauthplayground).
2. Click the gear icon (top right) → check **Use your own OAuth credentials** → enter your Client ID and Client secret.
3. In the left panel, find **Drive API v3** and select the scope:
   `https://www.googleapis.com/auth/drive.appdata`
4. Click **Authorize APIs** and sign in with your Google account.
5. Click **Exchange authorization code for tokens**.
6. Copy the **Refresh token**.

**Step 3 — Enter the secret in the app**

In the AI Edge Gallery skill settings, paste your credentials as a single JSON object:

```json
{"client_id":"YOUR_CLIENT_ID","client_secret":"YOUR_CLIENT_SECRET","refresh_token":"YOUR_REFRESH_TOKEN"}
```

### Try It Out

Here are some example prompts to get started:

- *"Record my great-grandmother: Mary Elizabeth O'Brien, born 15 March 1882 in Cork, Ireland. Died 4 January 1955 in Boston, Massachusetts."*
- *"Create a family group sheet for John Smith and Jane Doe, married 12 June 1910 in Chicago. Their children were Robert (b. 1911), Dorothy (b. 1914), and William (b. 1918)."*
- *"I found my ancestor in the 1870 US Federal Census on FamilySearch. Help me cite it properly."*
- *"I searched the Ellis Island passenger records for anyone named Kowalski arriving between 1890 and 1910 but found nothing. Log that."*
- *"What people do I have saved?"*
- *"Export the people I've recorded as GEDCOM."*
- *"What types of records would exist for someone who lived in rural Virginia in the 1840s?"*

## Project Structure

```
genealogy-research-assistant/
├── SKILL.md              # Skill definition (frontmatter + LLM instructions)
├── scripts/
│   └── index.html        # JavaScript skill logic (runs in webview)
├── LICENSE               # Apache 2.0
└── README.md             # This file
```

## How It Works

The skill has two layers:

1. **SKILL.md** provides the LLM with a genealogist persona and instructions for when and how to call the JavaScript functions.
2. **scripts/index.html** contains pure JavaScript that runs inside a sandboxed webview on the device. It generates formatted HTML cards, properly structured citations, and valid GEDCOM output — all without any network calls.

Because everything runs on-device, your family data never leaves your phone.

## Contributing

Contributions are welcome! Ideas for future enhancements:

- Ahnentafel (ancestor) numbering system support
- Pedigree chart HTML generation
- Support for additional source citation templates
- Localization for non-US record types and date formats
- Timeline generation for an individual's life events

Please open an issue or pull request.

## License

This project is licensed under the Apache License 2.0 — see [LICENSE](LICENSE) for details.

## Acknowledgments

- [Google AI Edge Gallery](https://github.com/google-ai-edge/gallery) for the Agent Skills platform
- [Evidence Explained](https://www.evidenceexplained.com/) by Elizabeth Shown Mills for citation methodology
- The genealogy community for decades of shared knowledge and standards
