# What Memento Taught Me About LLMs and Harnesses

An interactive, dependency-free slide deck that makes LLM context and tool calling visible. Each live demo uses OpenRouter chat completions and shows the chat beside the exact request and response produced by the browser harness.

![What Memento taught me about LLMs and harnesses](<What Memento (2000) taught me about how LLMs actually work (Really!).png>)

## What it demonstrates

1. **One request at a time** - a model receives the current API payload, not a private transcript.
2. **History is replayed** - a chat feels continuous because the harness sends earlier messages again.
3. **History can be dropped** - omit those messages and the apparent memory disappears.
4. **History can be rewritten** - the next response follows the supplied payload, even when it differs from the visible chat.
5. **Instructions are not tools** - asking a model to “call” something in prose still produces prose.
6. **Schemas enable tool calls** - a tool definition gives the model a structured response shape for the harness to execute.

## Run locally

There is no build step and no package installation.

```powershell
python -m http.server 8080
```

Open `http://localhost:8080`. Opening `index.html` directly also works in modern browsers, although a local server more closely matches GitHub Pages.

## Use the live demos

1. Open the deployed page, which uses the deployment key by default.
2. Optionally open **API settings** to override the key or model for the current tab.
3. Press the send button in any chat demo.

The default model is `google/gemma-4-26b-a4b-it:free`.
A local copy still requires a key from the [OpenRouter dashboard](https://openrouter.ai/settings/keys).

The browser sends `POST https://openrouter.ai/api/v1/chat/completions` with the standard bearer-token header plus OpenRouter's optional app-attribution headers. The chosen model must support tool calling for the structured tool-call demo.

## Deploy to GitHub Pages

The workflow at [`.github/workflows/pages.yml`](.github/workflows/pages.yml) validates the static app and deploys it on pushes to `main` or `master`.

1. Push this repository to GitHub.
2. Open **Settings > Pages**.
3. Set **Source** to **GitHub Actions**.
4. Add an Actions repository secret named `OPENROUTER_API_KEY`.
5. Run **Deploy static site to GitHub Pages** or push to the default branch.

The workflow rejects committed OpenRouter keys, requires exactly one placeholder, and embeds the repository secret only in the generated Pages artifact. The deployed key is recoverable from the public HTML and should be restricted accordingly.
