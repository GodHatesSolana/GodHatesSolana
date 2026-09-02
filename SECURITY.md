# Security Policy — GodHatesSolana ($GHS)

## Reporting a vulnerability
Please report security issues privately via DM to https://x.com/GodHatesSolana
(or open a **private** security advisory on GitHub). Do not open public issues
for exploitable bugs. We aim to acknowledge within 72h.

## Architecture (defense in depth)
- **Browser** — UI + public read-only data only.
- **Solana RPC** — read-only balance/mint lookups.
- **Supabase Realtime** — non-financial presence/chat.
- **Supabase Edge Function `set-stream-mode`** — the *only* write path for broadcast
  control. Verifies: authorized wallet, canonical message, Ed25519 signature,
  ±120s freshness, single-use nonce (anti-replay), whitelisted mode.
- **Postgres RLS** — `stream_control` is read-only for anon; writes require the
  service role (Edge Function). `used_nonces` has no anon access.

## Notes on keys
- The Supabase `publishable` key is designed to be public; safety comes from RLS.
- The Helius RPC key in the client is treated as public and is domain-restricted.
  It is rotated periodically.
- No private keys, seed phrases, or service-role keys are ever shipped to the browser.

## Not financial advice
$GHS is a meme/entertainment token. See the site disclaimer.
