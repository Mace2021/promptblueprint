# Prompt Blueprint

A single-file HTML app that turns a raw idea into a structured, reusable prompt template, then executes that template to generate the actual output — working code, content, or documents — with live token and cost tracking along the way.

**File:** `prompt-blueprint.html` — no build step, no install. Open it in a browser or view it inside a Claude.ai artifact panel.

---

## How it works

### Step 00 — Choose your model
Pick a provider and (where relevant) a model:

| Provider | Models | API key needed? |
|---|---|---|
| **Anthropic (Claude)** | Claude Sonnet | Only if running the file outside Claude.ai |
| **Google Gemini** | 2.5 Flash / 2.5 Pro | Always |
| **Hugging Face** | Any model ID you type — routes to Llama, Mistral, Qwen, DeepSeek, and other open models | Always |
| **OpenAI** | — | Unavailable — OpenAI blocks direct browser requests, so it needs a backend this file doesn't have |

Inside Claude.ai's artifact panel, Anthropic calls are authenticated automatically — no key required. Anywhere else, paste your own key in Step 00. Keys are held only in browser memory for that session; nothing is written to disk or saved into the file.

### Step 01 — Draft
Describe your idea in plain language and pick a template mode:
- **Full** — for complex or multi-step builds (apps, agents, workflows)
- **Lite** — for quick, low-token one-off requests

This fills out a structured template (Role, Objective, Context, Task, Output Format, Guidelines, Security & Safety) specific to your idea. The generated template is editable before you send it on.

### Step 02 — Build
Paste, edit, or upload (`.txt`/`.md`) a template, then generate the actual output. Results show in two tabs:
- **Preview** — renders live if the output is browser-runnable (HTML/CSS/JS); shows a clear "not previewable" notice with the raw code if it's in a language a browser can't execute (Python, Java, etc.), instead of silently duplicating the Code tab
- **Code** — the raw output, always, for copying or editing

Both template and output can be copied or downloaded, with the file extension chosen automatically based on content (`.html`, `.md`, or a matched code extension like `.py`).

---

## Token & cost tracking

Every call reports real token usage pulled directly from the API response — not a pre-run estimate. Cost is calculated using each provider's current published rate; Hugging Face is shown as "n/a" since its pricing depends on whichever underlying provider it routes a given model to.

## Handling long outputs

If a response gets cut off by a token limit, the app detects it automatically (via each provider's truncation signal) and continues the generation, stitching up to five chunks together into one complete result. You'll see a status message like "continuing (part 2 of up to 5)..." when this happens.

## Known limitations

- **Preview only renders HTML/CSS/JS.** Anything else (Python, Java, C++, etc.) generates correctly but can only be shown as code — there's no in-browser runtime for other languages.
- **OpenAI isn't supported.** It blocks direct browser calls; using it here would require a real backend to proxy requests.
- **Very large templates** (over ~60,000 characters) trigger a size warning before sending, since bigger requests are more likely to fail or time out.
- **No data persists** between sessions — refreshing the page clears any API key you've entered.

## Security notes

- API keys live only in a JS variable in memory for the current tab — never in localStorage, never written into the file, never sent anywhere but directly to the selected provider's API.
- The Build step treats the template's instructions as trusted (since you wrote or approved it) but treats any data pasted inside it as untrusted content, not commands.
