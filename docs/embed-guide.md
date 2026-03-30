# Chatbot Embed Guide

This guide explains how to embed the Flowise chatbot widget on a client's website using a single script tag.

---

## How to Get the Embed Snippet

1. Log in to your Flowise instance (`https://your-flowise-instance.onrender.com`)
2. Open the client's Chatflow
3. Click the **</>** (Embed) button in the top-right toolbar
4. Select the **Chatbot** tab
5. Copy the script snippet shown

The snippet looks like this (with your actual `chatflowid` and `apiHost`):

```html
<script type="module">
  import Chatbot from "https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js"
  Chatbot.init({
    chatflowid: "YOUR_CHATFLOW_ID_HERE",
    apiHost: "https://your-flowise-instance.onrender.com",
    theme: {
      button: {
        backgroundColor: "#3B81F6",
        right: 20,
        bottom: 20,
        size: 48
      },
      chatWindow: {
        title: "CLIENT BUSINESS NAME",
        welcomeMessage: "Hi! How can I help you today?",
        backgroundColor: "#ffffff",
        fontSize: 16
      }
    }
  })
</script>
```

**Before sending to client:** replace `CLIENT BUSINESS NAME` in the `title` field with the client's actual business name.

---

## WordPress

1. Log in to the WordPress admin dashboard
2. Go to **Appearance → Theme File Editor** (or use a plugin like **Insert Headers and Footers**)
3. To add via Insert Headers and Footers plugin (recommended — no theme editing required):
   - Install and activate the plugin from **Plugins → Add New**
   - Go to **Settings → Insert Headers and Footers**
   - Paste the embed snippet into the **Scripts in Footer** box
   - Click **Save**
4. Visit any page on the site — the chat widget will appear in the bottom-right corner

---

## Wix

1. Log in to Wix and open the site editor
2. Click **Add** → **Embed** → **Custom Code** (or use **Marketing Integrations → Custom Code** from the dashboard)
3. Click **+ Add Custom Code** in the site's **Head** or **Body** section
4. Paste the embed snippet and set placement to **Body - End**
5. Set **Apply to** → **All Pages**
6. Click **Apply** and publish the site

---

## Squarespace

1. Log in to Squarespace and open the site
2. Go to **Settings → Advanced → Code Injection**
3. Paste the embed snippet into the **Footer** box
4. Click **Save**
5. The chat widget will appear on all pages site-wide

---

## Plain HTML Test Page

Use this to verify the widget works before sending to the client:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Chatbot Test</title>
</head>
<body>
  <h1>Chatbot Widget Test</h1>
  <p>The chat button should appear in the bottom-right corner.</p>

  <!-- PASTE CLIENT EMBED SNIPPET HERE -->
  <script type="module">
    import Chatbot from "https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js"
    Chatbot.init({
      chatflowid: "YOUR_CHATFLOW_ID_HERE",
      apiHost: "https://your-flowise-instance.onrender.com",
      theme: {
        chatWindow: {
          title: "CLIENT BUSINESS NAME"
        }
      }
    })
  </script>
</body>
</html>
```

Open this file in any browser to test. No server required.

---

## Security Notes

- The Flowise instance is HTTPS-served by default on Render (SSL included)
- The embed snippet contains only a public chatflow ID — no API keys or secrets
- The chatbot only answers questions; it does not collect user data unless you add a lead capture node

---

## Troubleshooting

| Issue | Fix |
|---|---|
| Widget not appearing | Check browser console for CORS errors; ensure Flowise is running |
| "Chatflow not found" error | Verify `chatflowid` matches the chatflow in Flowise |
| Widget appears but no response | Check Flowise logs; Groq API key may be missing or invalid |
| Render free tier spun down | First message takes ~30s to wake the service; UptimeRobot keeps it warm |
