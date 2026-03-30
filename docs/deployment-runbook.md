# AI Chatbot Micro-Agency — Deployment Runbook

**Project:** AI Chatbot Micro-Agency
**Stack:** Flowise + n8n on Render free tier, Groq LLM, Supabase PostgreSQL
**Estimated setup time:** 45–60 minutes

---

## Prerequisites (Board Actions Required)

Before deploying, the board must create the following free accounts and collect credentials:

| Service | URL | Purpose | Free Tier |
|---------|-----|---------|-----------|
| Render | https://render.com | Host Flowise & n8n | 750 hrs/month per service |
| UptimeRobot | https://uptimerobot.com | Keep-alive pinging | 50 monitors, 5-min intervals |
| Supabase | https://supabase.com | PostgreSQL for n8n | 500MB, 2 projects |
| Groq | https://console.groq.com | Llama 3.3 70B LLM | 14,400 RPD |
| GitHub | https://github.com | Source repo | Free |

---

## Step 1 — GitHub Repo Setup

1. Create a new GitHub repository: `ai-chatbot-agency` (private recommended)
2. Push this project directory to that repo
3. Connect the Render services to this repo (optional — can also deploy directly via image)

---

## Step 2 — Supabase PostgreSQL Setup (for n8n)

1. Go to https://supabase.com → **New Project**
2. Project name: `n8n-agency`
3. Set a **strong database password** (save it — needed for Render env vars)
4. Region: choose closest to your users
5. After project created, go to **Settings → Database**
6. Note the **Host** value: `db.<project-ref>.supabase.co`
7. Note the **Password** you set in step 3

---

## Step 3 — Groq API Key Setup

1. Go to https://console.groq.com → Sign up / Log in
2. Navigate to **API Keys** → **Create API Key**
3. Name it: `flowise-agency`
4. Copy the key (starts with `gsk_...`) — save securely

---

## Step 4 — Deploy Flowise on Render (Story 1 — ITT-24)

### Option A: Deploy from Git Repo (recommended)
1. Go to https://dashboard.render.com → **New → Web Service**
2. Connect your GitHub repo
3. Render will detect `render.yaml` and auto-configure

### Option B: Deploy directly from Docker image
1. Go to https://dashboard.render.com → **New → Web Service**
2. Select **Deploy an existing image from a registry**
3. Image URL: `docker.io/flowiseai/flowise:latest`
4. Name: `flowise`
5. Region: Oregon (or closest)
6. Plan: **Free**

### Environment Variables to set in Render:
| Variable | Value |
|----------|-------|
| `PORT` | `3000` |
| `FLOWISE_USERNAME` | `admin` (or your choice) |
| `FLOWISE_PASSWORD` | Strong password (min 12 chars) |
| `FLOWISE_SECRETKEY_OVERWRITE` | Run `openssl rand -hex 16` → paste result |
| `DATABASE_PATH` | `/opt/render/project/.flowise` |
| `SECRETKEY_PATH` | `/opt/render/project/.flowise` |
| `LOG_PATH` | `/opt/render/project/.flowise/logs` |
| `BLOB_STORAGE_PATH` | `/opt/render/project/.flowise/storage` |
| `GROQ_API_KEY` | Your Groq key from Step 3 |

7. Click **Create Web Service**
8. Wait for deploy (~3-5 minutes)
9. Note the Render URL: `https://flowise-xxxx.onrender.com`
10. Test health: `curl https://flowise-xxxx.onrender.com/api/v1/ping` → should return `{"ping":"pong"}`
11. **Post the Render URL as a comment on ITT-24**

---

## Step 5 — Configure UptimeRobot Keep-Alive (Story 1 — ITT-24)

1. Go to https://uptimerobot.com → **Add New Monitor**
2. Monitor Type: **HTTP(s)**
3. Friendly Name: `Flowise Keep-Alive`
4. URL: `https://flowise-xxxx.onrender.com/api/v1/ping`
5. Monitoring Interval: **5 minutes**
6. Click **Create Monitor**

Repeat for n8n after Step 6:
- Friendly Name: `n8n Keep-Alive`
- URL: `https://n8n-xxxx.onrender.com/healthz`

---

## Step 6 — Deploy n8n on Render (Story 2 — ITT-25)

### Deploy from Docker image:
1. Go to https://dashboard.render.com → **New → Web Service**
2. Select **Deploy an existing image from a registry**
3. Image URL: `docker.io/n8nio/n8n:latest`
4. Name: `n8n`
5. Plan: **Free**

### Environment Variables to set in Render:
| Variable | Value |
|----------|-------|
| `DB_TYPE` | `postgresdb` |
| `DB_POSTGRESDB_HOST` | Supabase host from Step 2 |
| `DB_POSTGRESDB_PORT` | `5432` |
| `DB_POSTGRESDB_DATABASE` | `postgres` |
| `DB_POSTGRESDB_USER` | `postgres` |
| `DB_POSTGRESDB_PASSWORD` | Supabase password from Step 2 |
| `DB_POSTGRESDB_SSL_REJECT_UNAUTHORIZED` | `false` |
| `N8N_BASIC_AUTH_ACTIVE` | `true` |
| `N8N_BASIC_AUTH_USER` | `admin` (or your choice) |
| `N8N_BASIC_AUTH_PASSWORD` | Strong password |
| `N8N_PROTOCOL` | `https` |
| `NODE_FUNCTION_ALLOW_EXTERNAL` | `*` |

6. Click **Create Web Service**
7. After deploy, note the URL: `https://n8n-xxxx.onrender.com`
8. **Go back to Render env vars and set:**
   - `WEBHOOK_URL` = `https://n8n-xxxx.onrender.com/`
   - `N8N_HOST` = `n8n-xxxx.onrender.com`
9. Trigger a redeploy for the new env vars to take effect
10. Test: visit `https://n8n-xxxx.onrender.com` → should show Basic Auth login
11. **Post the n8n URL as a comment on ITT-25**

---

## Step 7 — Configure Groq in Flowise (Story 3 — ITT-26)

1. Open Flowise at `https://flowise-xxxx.onrender.com`
2. Log in with `FLOWISE_USERNAME` / `FLOWISE_PASSWORD`
3. Go to **Credentials** → **Add Credential**
4. Select **Groq**
5. API Key: paste your Groq key from Step 3
6. Save

### Create test chatflow:
1. Go to **Chatflows** → **Add New**
2. Name: `Test Groq Connection`
3. Drag in: **ChatGroq** node
   - Model: `llama-3.3-70b-versatile`
   - Temperature: `0.7`
   - Credential: Select your Groq credential
4. Drag in: **Chat Trigger** node → connect to ChatGroq
5. Save → Click **API Endpoint** → test with: `{"question": "Say hello"}`
6. Confirm response arrives within 5 seconds
7. **Post confirmation comment on ITT-26**

---

## Credentials Handoff to FS Agent

Once accounts are set up, share the following with the FS agent via secure channel (not issue comments):
- Render account: service URLs for Flowise and n8n
- Groq API key (already set as Render env var — FS just needs the URL)
- Flowise admin credentials (for chatflow configuration)
- n8n admin credentials (for workflow setup)

---

## Post-Deployment Checklist

- [ ] Flowise URL live and health endpoint responds
- [ ] Flowise admin UI requires login
- [ ] UptimeRobot monitor active for Flowise
- [ ] n8n URL live with Basic Auth login
- [ ] n8n connected to Supabase (check Settings → Database in n8n)
- [ ] UptimeRobot monitor active for n8n
- [ ] Groq LLM test chatflow responds correctly
- [ ] All credentials stored as env vars (none hardcoded)
