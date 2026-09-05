# $GHS — Security Overview & Audit Remediation

> Independent technical review of the public code base, plus the fixes applied.
> Published for transparency. This is a community project; see the site disclaimer.

## Scope & method
Reviewed: the public site, the 3D Church (`church.html`, incl. `?watch=1`), the
Solana balance read path, the Supabase Realtime usage, and the broadcast admin
control. On-chain forensic depth (full holder graph, historical transfers) is out
of scope and should be verified live via the tools linked on the Transparency page.

## Key result
- **No custom Solana program.** $GHS is a standard SPL mint launched on Pump.fun.
  The interface only *reads* balances via RPC — there is no on-chain code in this
  project able to move balances, mint, or drain funds. Verify authorities live:
  https://rugcheck.xyz/tokens/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump
- The main historical risk was **application-level**: the admin/broadcast control
  trusted a client-side check. That has been re-architected server-side.



## Token safety — verified on-chain (RPC)
Checked directly against the mint `3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump`:
- **Mint authority: `null`** — supply is fixed (~999.5M, 6 decimals); no inflation possible.
- **Freeze authority: `null`** — accounts cannot be frozen or blacklisted.
- **Program: Token-2022**, extensions = `metadataPointer` + `tokenMetadata` only.
  No `transferFeeConfig` (no tax), no `transferHook`, no `permanentDelegate` — i.e. no hidden seize/redirect vectors.
- **Metadata update authority: `null`** — name/symbol/URI are immutable.
- Royalties: 0%. Re-verify anytime on RugCheck / Solsniffer / Solscan.

## Findings & remediation

| # | Finding | Severity | Status |
|---|---------|----------|--------|
| 1 | Broadcast control written directly browser → DB with permissive RLS | Critical | **Fixed** — writes go through the `set-stream-mode` Edge Function which verifies an Ed25519 wallet signature; the table is read-only for anon. |
| 2 | No replay/freshness proof on the signed command | Critical | **Fixed** — canonical message, ±120s freshness, single-use nonce table. |
| 3 | RPC provider key shipped in client JS | Medium | **Fixed** — a server-side `rpc` Edge Function proxies read-only methods; the key lives in a secret and is rotatable. |
| 4 | Supabase RLS too permissive | Medium | **Fixed** — anon can only SELECT `stream_control`; writes require the service role; `used_nonces` has no anon access. |
| 5 | Missing HTTP security headers | Low | **Fixed** — `_headers`: HSTS, X-Content-Type-Options, X-Frame-Options, Referrer-Policy, Permissions-Policy (+ ready-to-enable CSP). |
| 6 | Roadmap features presented as if live | Low | **In progress** — labeled PLANNED; off-chain, signature-based voting is being implemented. |
| 7 | No public disclosure process / CI | Low | **Fixed** — `SECURITY.md`, `/.well-known/security.txt`, and a secret-scanning GitHub Action. |
| 8 | Charity claim could imply a partnership | Low | **Fixed** — 50% is auto-routed to St. Jude via Pump.fun's Creator Fee Sharing (verifiable on-chain, first proof published); explicit non-affiliation kept. |

## Architecture (target, now implemented)
1. Browser — UI + public read-only data only.
2. RPC proxy (Edge Function) — hides the provider key, read methods allowlist.
3. Supabase Realtime — non-financial presence/chat.
4. Edge Function `set-stream-mode` — the only admin write path; verifies wallet
   signature, freshness and nonce.
5. Postgres RLS — public read, service-role writes only.
6. Netlify — security headers.
7. Separate wallets — creator-fee / donation / treasury kept distinct.

## Verify it yourself
Token safety: RugCheck, Solsniffer, GoPlus, Solscan (links on `/transparency.html`).
Disclosure: `/.well-known/security.txt`. Report privately: https://x.com/GodHatesSolana
