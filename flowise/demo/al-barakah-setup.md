# Al-Barakah Restaurant Demo Setup

## Overview

Live demo chatbot for client outreach calls. Shows a working restaurant AI assistant with menu FAQ, reservations, delivery info, and human handoff.

## Setup Steps

### 1. Create the Chatflow in Flowise

1. Import `rag-chatflow-template.json` into Flowise (or duplicate an existing chatflow)
2. Rename to `Al-Barakah Demo`
3. Configure the nodes:

   **Faiss Vector Store** node:
   - `basePath`: `/root/.flowise/vectorstore/al-barakah`

   **Conversational Retrieval QA Chain** node — system prompt:
   ```
   You are a helpful assistant for Al-Barakah Restaurant. Answer questions only using the provided documents. If the answer is not in the documents, say: "I don't have that information. Please contact us at (555) 876-5432 or info@albarakah.com."

   Be concise, friendly, and professional. Do not make up information.

   ## Human Handoff
   If the user asks to speak to a human, real person, agent, or says "help me" in a way that indicates they want human assistance, respond with:
   "I'd be happy to connect you with our team! To make sure someone gets back to you quickly, could you please share:
   1. Your name
   2. Your phone number or email

   Once you provide those details, our team at Al-Barakah will reach out shortly."

   After the user provides their contact information, confirm receipt and say: "Thank you! Our team at Al-Barakah Restaurant will contact you soon. Is there anything else I can help with in the meantime?"
   ```

   **Plain Text** node:
   - Paste the entire contents of `al-barakah-faq.txt`

4. Click **Upsert** to index the documents
5. Copy the Chatflow ID from the URL bar

### 2. Update the Demo Page

1. Open `index.html`
2. Replace `YOUR_CHATFLOW_ID_HERE` with the actual chatflow ID
3. Replace `https://your-flowise-instance.onrender.com` with the actual Flowise URL

### 3. Host the Demo Page

**Option A: Render Static Site (recommended)**
- Create a new Static Site on Render
- Point to the `flowise/demo/` directory
- Free tier, HTTPS included

**Option B: Netlify**
- Drag and drop `index.html` to Netlify
- Free tier, instant deploy

**Option C: Local testing**
- Open `index.html` directly in any browser

### 4. Test the Demo

Verify these scenarios work:

| Test | Ask | Expected |
|---|---|---|
| Hours | "What are your Friday hours?" | 11:00 AM - 11:00 PM |
| Menu | "How much is the Mixed Grill?" | $28 |
| Vegetarian | "Vegetarian options?" | Vegetarian Moussaka ($16), Falafel ($9/$10 wrap), Fattoush ($8) |
| Delivery | "Do you deliver?" | Yes, 5-mile radius, $3 fee (free over $40), 30-45 min |
| Reservations | "Book for 12 people?" | Call or book online, groups 10+ need set menu from $35/person |
| Handoff | "Talk to a real person" | Asks for name and phone/email |
| Halal | "Is the food halal?" | Yes, 100% halal certified |

### 5. Share the Demo URL

Post the live demo URL as a comment on the Paperclip issue for PO review.
