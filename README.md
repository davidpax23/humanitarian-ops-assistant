# Humanitarian Operations AI Assistant

A production-quality AI tool for IRC staff to access, interpret, and act on complex operational documents — field reports, policies, and program documentation.

Built as a portfolio artifact for the AI Operations Manager role (JR00002781), IRC External Relations.

---

## Architecture

```
Browser (public/index.html)
        |
        | POST /api/chat  (no API key in browser)
        v
Vercel Serverless Function (api/chat.js)
        |
        | x-api-key: ANTHROPIC_API_KEY (env var — never exposed)
        v
Anthropic Claude API
```

The API key lives only in Vercel's environment variables. Users never see it.

---

## Deploy to Vercel (5 minutes)

### Step 1 — Push to GitHub

```bash
# In this folder:
git init
git add .
git commit -m "Initial deploy"
gh repo create humanitarian-ops-assistant --public --push
# or: git remote add origin https://github.com/YOUR_USERNAME/humanitarian-ops-assistant.git && git push -u origin main
```

### Step 2 — Import to Vercel

1. Go to https://vercel.com and sign in (free account works)
2. Click **Add New Project**
3. Import your GitHub repo
4. Leave all build settings as defaults — click **Deploy**

### Step 3 — Add your API key

1. In your Vercel project, go to **Settings → Environment Variables**
2. Add:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** `sk-ant-...` (your Anthropic API key)
   - **Environment:** Production (and Preview if you want)
3. Click **Save**
4. Go to **Deployments** → click the three dots on your latest deploy → **Redeploy**

### Step 4 — Share

Vercel gives you a URL like `https://humanitarian-ops-assistant.vercel.app`

That URL works for anyone — no login, no API key, no setup. Just send the link.

---

## Local Development

```bash
npm install -g vercel
vercel dev
```

Then open http://localhost:3000 — the proxy works locally too.

Or set `ANTHROPIC_API_KEY` in a `.env.local` file:
```
ANTHROPIC_API_KEY=sk-ant-...
```

---

## Project Structure

```
├── api/
│   └── chat.js          ← Vercel serverless function (API proxy)
├── public/
│   └── index.html       ← Full frontend app
├── vercel.json          ← Routing config
├── package.json
└── README.md
```

---

## Workflows

| Workflow | Use Case |
|---|---|
| 📋 Field Report Summary | Extract outputs, issues, risk flags from field reports |
| 📜 Policy Interpretation | Plain-language answers to compliance questions |
| 🔤 Plain Language | Translate complex documents for non-specialist staff |
| ⚡ Decision Support | Tiered checklists for urgent field decisions |

---

## Cost Control

To prevent unexpected charges, set a spend limit in your Anthropic console:
https://console.anthropic.com/settings/limits

For a demo/interview context, $5–10 is more than enough.

---

## Security Notes

- The API key is stored as a Vercel environment variable — never in the frontend code
- CORS is set to `*` for demo purposes — in production, restrict to your domain
- No authentication layer — add one (e.g. Vercel Password Protection) if needed for sensitive demos
