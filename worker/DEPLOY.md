# Deploying the chat backend

This runs the public GitHub Pages chat widget on Cloudflare Workers AI (open-source
models, no external API key needed). Run these yourself in your own terminal —
none of this needs to go through Claude, and `wrangler login` opens a browser window.

## 1. One-time setup

```bash
cd worker
npm install
wrangler login          # opens a browser, log in with (or create) your Cloudflare account
```

## 2. Create the rate-limit KV namespace

```bash
wrangler kv namespace create RATE_LIMIT
```

This prints something like:

```
[[kv_namespaces]]
binding = "RATE_LIMIT"
id = "abcd1234..."
```

Copy the `id` value into `wrangler.toml`, replacing `REPLACE_WITH_KV_NAMESPACE_ID`.

## 3. Deploy

```bash
wrangler deploy
```

This prints your Worker's live URL, something like:

```
https://agentic-roadmap-chat.<your-subdomain>.workers.dev
```

## 4. Send me that URL

Paste the URL back in chat (it's a public endpoint, not a secret) and I'll wire it
into the roadmap page's chat widget so the GitHub Pages copy calls it.

## Notes

- No API key is stored anywhere — Workers AI runs on your Cloudflare account's own
  free allocation of "neurons" (currently a daily free tier; check your dashboard's
  Workers AI usage page if you want to confirm current limits).
- The Worker only accepts requests from `https://sri18386.github.io` (CORS-locked) and
  rate-limits each IP to 15 messages/minute — both enforced server-side in `src/index.js`.
- To swap models later, change the `MODEL` constant in `src/index.js` to any chat
  model listed in the Cloudflare Workers AI model catalog, then `wrangler deploy` again.
