## AI Elements — Next.js AI chat UI

This is a modern chat UI built with Next.js App Router and the AI SDK. It streams model responses with reasoning and sources, supports web search, and lets users switch models.

### Features
- **Streaming responses** with the AI SDK `streamText`
- **Reasoning and sources** panes (toggleable in the UI)
- **Model picker** (e.g. `openai/gpt-4o`, `deepseek/deepseek-r1`)
- **Web search** toggle that routes to `perplexity/sonar`
- **Dark/light theme** with system preference

### Tech stack
- **Next.js 15** (App Router)
- **React 19**
- **AI SDK 5** (`ai`, `@ai-sdk/react`)
- **Radix UI** + small UI primitives in `components/ui`

---

## Quickstart

### 1) Prerequisites
- Node.js 18.17+ (Node 20+ recommended)
- npm, yarn, pnpm, or bun

### 2) Install dependencies
```bash
pnpm install
# or: npm install / yarn / bun install
```

### 3) Configure environment variables
Create a local env file and set the keys you plan to use.
```bash
cp .env.example .env.local
```

You can run via an AI Gateway (recommended) or connect directly to providers.

#### Option A: AI Gateway (recommended)
Set these variables:

```bash
AI_GATEWAY_URL=    # e.g. your AI Gateway base URL
AI_GATEWAY_API_KEY=
```

Notes:
- The app uses model IDs like `openai/gpt-4o`, `deepseek/deepseek-r1`, and `perplexity/sonar`.
- Ensure your gateway is configured to route these model IDs to the corresponding providers.

#### Option B: Direct provider keys
Set the keys for the models you intend to use:

```bash
OPENAI_API_KEY=
DEEPSEEK_API_KEY=
PERPLEXITY_API_KEY=
# Optional advanced overrides if needed:
# OPENAI_BASE_URL=
# DEEPSEEK_API_BASE=
# PERPLEXITY_BASE_URL=
```

Security tips:
- Do not prefix server-only secrets with `NEXT_PUBLIC_`.
- Keep real secrets out of git. Use `.env.local` for local dev; it’s gitignored.
- If any secret was committed previously (e.g. in `.env`), rotate it.

### 4) Start the app
```bash
pnpm dev
# or: npm run dev / yarn dev / bun dev
```
Open `http://localhost:3000`.

---

## Scripts
```bash
pnpm dev     # start dev server (Turbopack)
pnpm build   # production build
pnpm start   # start production server
pnpm lint    # run ESLint
```

---

## How it works
- **Client**: `components/ai-chat.tsx` uses `useChat` from `@ai-sdk/react`. It lets users select a model and toggle web search. Submissions are sent to the API with `{ model, webSearch }`.
- **Server**: `app/api/chat/route.ts` calls `streamText` and streams tokens back. When web search is on, it uses the `perplexity/sonar` model; otherwise it uses the selected model. The response includes reasoning and sources, which the UI renders.

You can add more models by editing the `models` array in `components/ai-chat.tsx`.

---

## Deployment
- Deploy on Vercel or any Node host. Ensure the same env vars are set in the hosting environment.

---

## Troubleshooting
- 401 or 403 errors: check that your `AI_GATEWAY_API_KEY` or provider API keys are set and valid.
- Model not found: ensure the gateway or provider supports the model ID you selected.
- No streaming: verify your host supports streaming responses and that the route isn’t buffered by a proxy.

---

## License
MIT (or your preferred license)

