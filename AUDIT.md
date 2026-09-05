# Technical Security Assessment — GodHatesSolana ($GHS)

**Document type:** technical security assessment of the $GHS stack (token, website, backend, and community-rewards mechanism).
**Date:** 3 September 2026.
**Scope:** security only — on-chain token safety, custody model, application/backend controls, and rewards integrity. Market data is out of scope by design.

---

## 1. Summary

$GHS is a standard Solana **Token-2022** launched on Pump.fun, with **no custom, fund-moving smart contract** in the trading path. On-chain authorities are renounced, the token carries no fee/hook/delegate mechanisms, and the surrounding website and backend are built on a **non-custodial, signature-authenticated, least-privilege** architecture. Every security-relevant claim below is independently re-verifiable on-chain.

**Overall posture: strong.** No mechanism exists by which the token or the site can seize, freeze, or redirect a holder's assets.

---

## 2. Token contract security (verified on-chain)

| Control | Result |
|---|---|
| Mint authority (inflation) | **Renounced (null)** — supply cannot be increased |
| Freeze authority (blacklist/freeze) | **Null** — balances cannot be frozen |
| Metadata update authority | **Null** — name / symbol / URI are immutable |
| Buy / sell tax | **0%** |
| Transfer hook | **None** |
| Permanent delegate | **None** |
| Token royalties | **0%** |
| Standard / extensions | Token-2022, **metadata extension only** |

**Interpretation:** the classic red flags (open mint, freeze, hidden tax, transfer hook, permanent delegate) are all **absent**. The token behaves as a plain, immutable SPL asset.

---

## 3. Custody model — non-custodial by design

- Private keys **never leave the user's wallet** (Phantom / Solflare). The site holds no keys and takes no custody.
- The 3D church authenticates users with a **read-only signed message**, never a transfer approval. Connecting the wallet cannot move funds.
- The owner funding tool (`/fund.html`) builds a transfer that is **signed inside the user's own wallet**; no key or secret is transmitted to any server.

---

## 4. Application & backend security controls (in place)

- **Authenticated privileged actions.** Broadcast/admin commands are validated **server-side** in isolated Edge Functions using **Ed25519 signature verification**, a **single-use nonce (anti-replay)**, and a **±120-second freshness window**. A command cannot be forged, replayed, or issued from the browser.
- **Server-side RPC proxy.** All chain reads pass through a proxy with a **method allow-list**; RPC credentials live only on the server, never in client code.
- **Least-privilege database.** Row-Level Security is enabled: public data is read-only to the public, and **all writes go through authenticated Edge Functions** — the anonymous client cannot write privileged tables.
- **Transport & browser hardening.** Content-Security-Policy, HSTS, X-Content-Type-Options, X-Frame-Options, Referrer-Policy and related headers are enforced.
- **Responsible disclosure & supply chain.** A `/.well-known/security.txt` (RFC 9116) contact is published; continuous **secret scanning** and **dependency monitoring** run in CI.

---

## 5. Community-rewards integrity (Manna)

- Weekly rewards are distributed as a **strictly equal share** to the verified congregation, and the distribution's **Merkle root is recorded on-chain**, so any participant can independently verify the payout set and amount.
- The mechanism is being moved to an **immutable on-chain program** (`manna_vault`): the equal-split rule is **enforced by the program itself**, distribution is **authority-gated**, and each distribution emits an on-chain proof (day + Merkle root). The program is **deployed and validated on devnet** ahead of a mainnet release, with the source published for open review.

---

## 6. Charity transparency

- 50% of creator fees are routed to St. Jude via Pump.fun's native fee-sharing to a listed charity recipient wallet, **verifiable on-chain**.
- First distribution proof (30 Aug 2026): transaction `5QnxLmhSXJPeNeszZRs8T8UWTaxbr9x8xE8rk4QMSg5tZo9nmFAYz1fuuTAz4FacEsKVeFQ7tTXCqT8jTL6RvWQQ`.
- $GHS is **not affiliated with** St. Jude Children's Research Hospital.

---

## 7. Official addresses

| Role | Address |
|---|---|
| Token mint (CA) | `3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump` |
| Creator-fee wallet (verified deployer) | `FWvajde3ghsZiCYMYoDNZkqmeicG59s1S8jow6sC1RGT` |
| Charity recipient — St. Jude (via Pump.fun) | `DApXFR4nXGp2Es1SJDkVSRX34bNiVMqXK65iAr2CUYhG` |
| Manna distributor (community rewards) | `B4UrgQdGzNziECTAQNJeNuGvsV1Q8n8atsfRXAbuWACR` |

Any address not listed here should be treated as impersonation.

---

## 8. Independent re-verification

Anyone can re-run these automated third-party checks on the mint:

- RugCheck — https://rugcheck.xyz/tokens/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump
- GoPlus — https://gopluslabs.io/token-security/solana/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump
- Solsniffer — https://solsniffer.com/scanner/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump
- Solscan — https://solscan.io/token/3T4gdB2D4FbnPkh2vELdUrnRcZKAzB5z9QUpFechpump

---

## 9. Security scorecard (security axes only)

| Axis | Rating |
|---|---|
| Token authorities renounced (mint/freeze/metadata) | Strong |
| No tax / hook / permanent delegate | Strong |
| Non-custodial design (no key custody, sign-message only) | Strong |
| Backend access control (Ed25519 + anti-replay + least-privilege) | Strong |
| On-chain verifiability & transparency | Strong |
| Rewards integrity (equal split + on-chain Merkle; moving fully on-chain) | Strong |

---

## Disclaimer

$GHS / GodHatesSolana is a satirical, community, entertainment token with no intrinsic value and no promise of return. Nothing here is financial, legal, or spiritual advice — do your own research and verify on-chain. Not affiliated with, sponsored by, or endorsed by Solana, any religion, or St. Jude Children's Research Hospital. Roadmap items are aspirations, not guarantees.
