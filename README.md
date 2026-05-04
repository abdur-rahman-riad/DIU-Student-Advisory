# Student Advisory System
DIU · Batch 67 · CSE — Powered by Gemini 2.5 Flash

---

## Project Structure

```
student-advisory/
├── public/
│   └── index.html          ← Main frontend (all student data embedded)
├── netlify/
│   └── functions/
│       └── gemini.js       ← Serverless API proxy (hides your Gemini key)
├── .env.example            ← Environment variable template
├── .gitignore
├── netlify.toml            ← Netlify build config
├── package.json
└── README.md
```

---

## How It Works

- All student data (62 students, 6 CSV sources) is **hardcoded in `index.html`** and loaded into session memory on page load.
- When a student ID is searched, only that student's data is scoped to the AI.
- AI chat goes through **`/.netlify/functions/gemini`** — your Gemini API key never reaches the browser.
- Changing the student resets chat history and AI context completely.
- Refresh/close clears all session state (no localStorage used).

---

## Setup & Deployment

### Step 1 — Get a Gemini API Key
1. Go to [https://aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
2. Create a new API key for **Gemini 2.5 Flash**
3. Copy the key

---

### Step 2 — Deploy to Netlify

**Option A: Drag & Drop (Fastest)**
1. Go to [https://app.netlify.com](https://app.netlify.com)
2. Log in → click **"Add new site"** → **"Deploy manually"**
3. Zip the entire `student-advisory/` folder and drag it onto the deploy area
4. After deploy → go to **Site settings → Environment variables**
5. Add: `GEMINI_API_KEY` = `your_actual_key_here`
6. Trigger a redeploy from **Deploys → Trigger deploy**

**Option B: GitHub + Netlify (Recommended for updates)**
1. Push this folder to a GitHub repo
2. Connect repo on Netlify: **Add new site → Import from Git**
3. Build settings:
   - Build command: *(leave empty)*
   - Publish directory: `public`
   - Functions directory: `netlify/functions`
4. Add environment variable `GEMINI_API_KEY` in Netlify dashboard
5. Deploy → Done

---

### Step 3 — Local Development

```bash
# Install Netlify CLI
npm install

# Create .env file
cp .env.example .env
# Edit .env and add your real GEMINI_API_KEY

# Run locally (functions + frontend together)
npx netlify dev
# Opens at http://localhost:8888
```

---

## Vercel Alternative

Vercel doesn't natively support Netlify functions, but you can adapt:

1. Create a `api/gemini.js` file with the same logic using Vercel's Edge/Serverless format
2. Add `GEMINI_API_KEY` in Vercel Project → Settings → Environment Variables
3. Update the fetch URL in `index.html` from `/.netlify/functions/gemini` → `/api/gemini`

---

## Updating Student Data

The student data lives in `public/index.html` inside the `<script>` block:
- `RAW_SUMMARY` — core student info
- `SGPA_DATA` — semester GPA trends
- `PAYMENT_STATUS` — payment status per student
- `ISSUES` — flagged issues per student
- `SEMESTER_RESULTS` — detailed course results (key students)

To add/update: edit the relevant array and redeploy.

---

## Features

- 🔍 Live search + filter by name or ID
- 📊 CGPA, credits, payment, SGPA trend, guardian info
- ⚠️ Auto-detected issue flags
- 💬 AI chat scoped strictly to selected student
- 🔒 API key hidden server-side via Netlify function
- 📱 Fully responsive (mobile + desktop)
- 🧹 Session-only — clears on refresh
