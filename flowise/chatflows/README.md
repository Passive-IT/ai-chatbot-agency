# Flowise Chatflow Templates

Pre-built Flowise chatflow JSON files for import into the deployed Flowise instance.

## Available Chatflows

### `test-groq-connection.json`
**Purpose:** Validates Groq LLM integration (Story 3 — ITT-26)

**Components:**
- **ChatGroq** node → `llama-3.3-70b-versatile` model, temperature 0.7, streaming enabled
- **Conversation Chain** node → system prompt configured, wired to ChatGroq

**How to import:**
1. Open Flowise at `https://flowise-xxxx.onrender.com`
2. Go to **Chatflows** → click the **Import** button (upload icon)
3. Select `test-groq-connection.json`
4. Open the chatflow → click the **ChatGroq** node → assign your Groq credential
5. Save → click the chat bubble icon to test
6. Send: `"Say hello"` — expect a response within 5 seconds

**Groq credential setup (one-time):**
1. In Flowise sidebar → **Credentials** → **Add Credential**
2. Type: **Groq**
3. API Key: paste your `GROQ_API_KEY` value
4. Save

## Quota Limits & Fallback

| Model | RPD | TPM | Use Case |
|-------|-----|-----|----------|
| `llama-3.3-70b-versatile` | 14,400 | 6,000 | Default — best quality |
| `llama-3.1-8b-instant` | 14,400 | 20,000 | Fallback — higher TPM quota |

**If `llama-3.3-70b-versatile` hits quota (429 errors):**
- Open the chatflow → click ChatGroq node
- Change Model Name to `llama-3.1-8b-instant`
- Save and redeploy
