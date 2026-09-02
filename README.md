# ✝ GodHatesSolana — $GHS

[![Security policy](https://img.shields.io/badge/security-policy-9945FF)](./SECURITY.md)
[![RugCheck](https://img.shields.io/badge/RugCheck-verify%20live-14F195)](https://rugcheck.xyz/tokens/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump)
[![Secret scan](https://github.com/GodHatesSolana/GodHatesSolana/actions/workflows/security.yml/badge.svg)](../../actions/workflows/security.yml)
[![No custom contract](https://img.shields.io/badge/smart%20contract-none%20(standard%20SPL)-f5c542)](https://solscan.io/token/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump)


> A spite-fueled memecoin for heretics with a bag. Every green candle angers a deity — so we pump it anyway.

**$GHS** is a community memecoin on Solana / Pump.fun. Holders hold the power:
weighted votes, royalties and rewards through a **Divine Hierarchy** (Archangels →
Congregation). 100% fun, 0% divine approval.

## Links
- 🌐 Website: https://godhatessolana.netlify.app
- 🐦 X: https://x.com/GodHatesSolana
- 🛒 Pump.fun: https://pump.fun/coin/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump
- 📜 Contract (mint): `3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump`

## ⛪ The Church of $GHS
A real-time, top-down 3D church built into the site.
- **Repent** — connect a Solana wallet (Phantom / Solflare) that holds $GHS and
  walk in. The heavier your bag, the larger you stand before God. Ranks by holding:
  Archangel · Saint · Prophet · Disciple · Congregation.
- **Observe** — a wallet-free spectator view that shows the whole congregation
  live, from above.

Multiplayer presence, chat and emotes run on Supabase Realtime; balances are read
on-chain from Solana.

## 🤍 The Offering
**50% of creator fees are donated to St. Jude Children's Research Hospital.**
*This project is an independent community effort and is not affiliated with,
endorsed by, or sponsored by St. Jude or ALSAC.*

## Tech
Single-file HTML/CSS/JS site · Three.js (3D church) · Supabase Realtime
(multiplayer) · Solana JSON-RPC (on-chain balances). Hosted on Netlify.

## 🔐 Security & Transparency
- **Verify the token live:** [RugCheck](https://rugcheck.xyz/tokens/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump) · [Solsniffer](https://www.solsniffer.com/scanner/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump) · [GoPlus](https://gopluslabs.io/token-security/solana/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump) · [Solscan](https://solscan.io/token/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump)
- **Transparency page:** [`/transparency.html`](https://godhatessolana.netlify.app/transparency.html) — architecture, audit status, official wallets, Proof of Charity.
- **Audit report:** [`/security/AUDIT.md`](./security/AUDIT.md)
- **Disclosure:** [`SECURITY.md`](./SECURITY.md) · [`/.well-known/security.txt`](./.well-known/security.txt)
- **App security:** RPC key server-side (proxy), admin control verified by wallet signature in an Edge Function, Postgres RLS (public read, no anon writes), security headers.

## ⚠️ Disclaimer
$GHS is a meme/entertainment token with **no intrinsic value and no expectation
of profit**. Crypto is extremely volatile and most memecoins go to zero. Nothing
here is financial advice. Only spend what you can afford to lose entirely. Do your
own research.
