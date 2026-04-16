# 💗 Zhenox

> **Your personas, tokenized.**
> Turn social profiles into personal AI agents — chat with idols, mentors, anime crushes, or your own custom creations. On-chain, on BNB, on brand.

[![Chain: BNB](https://img.shields.io/badge/chain-BNB-F0B90B?style=flat-square)](https://bscscan.com)
[![Token: ZHNX](https://img.shields.io/badge/token-ZHNX-EC4899?style=flat-square)](#tokenomics)
[![License: MIT](https://img.shields.io/badge/license-MIT-0B0E11?style=flat-square)](#license)
[![i18n](https://img.shields.io/badge/i18n-EN%20%7C%20%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-EC4899?style=flat-square)](#)

---

## What is Zhenox?

**Zhenox** is an AI character chat platform built on **BNB Chain** where any social profile — your favorite creator, a fictional idol, an anime waifu, a mentor, or a mystery stranger — can be spun up as a **trainable, tokenized AI agent**. Every persona lives on-chain, every chat is access-gated by **$ZHNX**, and every conversation is routed through the right model for the right vibe.

Think of it as **Character.AI meets Web3**, but with proper economics, proper i18n, and proper safety rails. Creators train personas, holders unlock them, and the token decides what you can see. No VC gatekeeping, no bland corporate chatbots — just vibes, lore, and a hard-coded rule that says: **NSFW never touches the Core inference pipeline.**

---

## Features

- **Persona Forge** — Create, train, and deploy AI characters from scratch or by importing a social profile.
- **Dual-LLM Routing** — Safe chat through **Zhenox Core**, spicy chat through **Zhenox Eros**, with **Zhenox Guardian** filtering anything that crosses the line.
- **Image Generation** — SFW portraits via **Zhenox Imagen**, NSFW art via **Zhenox Canvas** — strictly siloed.
- **Token-Gated Tiers** — Hold $ZHNX to unlock Unlimited, NSFW, or VIP access.
- **Multi-Genre Library** — Idols, mentors, friends, anime, roleplay, mystery, 18+ flirty.
- **Wallet-Native Auth** — RainbowKit + wagmi, no passwords, no email spam.
- **Bilingual UI** — Full English + 简体中文 parity out of the box.
- **On-Chain Persona Registry** — Every character is indexed via Supabase and referenced on BSC.
- **Creator Royalties** — Persona authors earn ZHNX from chat volume.
- **Mobile-First UI** — Pink-on-dark aesthetic, tuned for thumbs.

---

## Tech Stack

| Layer          | Technology                                      |
| -------------- | ----------------------------------------------- |
| Framework      | Next.js 16 (App Router, RSC)                    |
| Language       | TypeScript (strict mode)                        |
| Styling        | Tailwind CSS 4                                  |
| Database       | Supabase (Postgres + Realtime + Auth)           |
| Wallet         | wagmi + RainbowKit                              |
| Chain          | BNB Smart Chain (BEP-20)                        |
| LLM (Safe)     | Zhenox Core (proprietary safe-chat engine)      |
| LLM (NSFW)     | Zhenox Eros (proprietary adult-chat engine)     |
| LLM (Filter)   | Zhenox Guardian (rules + classifier)            |
| Image (SFW)    | Zhenox Imagen (proprietary image engine)        |
| Image (NSFW)   | Zhenox Canvas (proprietary NSFW image engine)   |
| i18n           | next-intl (EN + zh-CN)                          |
| Deployment     | Vercel / Coolify                                |

---

## Quick Start

```bash
# Clone
git clone https://github.com/ZhenoxApp/zhenox.git
cd zhenox

# Install
pnpm install

# Configure env
cp .env.example .env.local
# → fill in Supabase, LLM provider, image provider, WalletConnect keys

# Dev
pnpm dev
```

Required `.env.local` keys:

```env
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
LLM_CORE_CREDENTIALS=
LLM_EROS_API_KEY=
IMAGE_PROVIDER_TOKEN=
NEXT_PUBLIC_WC_PROJECT_ID=
NEXT_PUBLIC_ZHNX_CONTRACT=0x...
```

App runs at `http://localhost:3000`. Production lives at **zhenox.com**.

---

## Project Structure

```
zhenox/
├── app/
│   ├── [locale]/           # i18n routes (en, zh)
│   │   ├── (marketing)/    # Landing, pricing, about
│   │   ├── chat/           # Persona chat UI
│   │   ├── forge/          # Persona creation studio
│   │   └── vault/          # Wallet + token-gated content
│   └── api/
│       ├── llm/route.ts    # Core/Eros/Guardian router
│       └── image/route.ts  # Zhenox Imagen + Canvas dispatcher
├── components/
│   ├── chat/
│   ├── persona/
│   └── wallet/
├── lib/
│   ├── llm/                # Zhenox Core + Eros + Guardian
│   ├── image/              # SFW + NSFW pipelines
│   ├── supabase/
│   └── chain/              # BSC contract helpers
├── messages/               # en.json, zh.json
├── public/
└── docs/                   # You are here
```

---

## LLM Routing Flow

```
                    ┌─────────────────────┐
  user message ───▶ │  Zhenox Guardian    │
                    │  (content classify) │
                    └──────────┬──────────┘
                               │
                 ┌─────────────┼─────────────┐
                 ▼             ▼             ▼
             [ SAFE ]      [ NSFW ]      [ BLOCKED ]
                 │             │             │
                 ▼             ▼             ▼
        ┌────────────────┐ ┌──────────────┐ ┌──────────┐
        │  Zhenox Core   │ │ Zhenox Eros  │ │  Reject  │
        │ (safe engine)  │ │(adult engine)│ │ + Notice │
        └───────┬────────┘ └──────┬───────┘ └──────────┘
                │                 │
                ▼                 ▼
       ┌─────────────────────────────────┐
       │   Tier check (ZHNX balance)     │
       │   basic / unlimited / nsfw / vip│
       └────────────────┬────────────────┘
                        ▼
                   stream to user
```

**Hard rule:** NSFW traffic is **physically prevented** from reaching the Core inference pipeline. Separate clients, separate keys, separate code paths.

---

## Tokenomics

**$ZHNX** — BEP-20 on BNB Smart Chain. Total supply **1,000,000,000**.

| Tier          | ZHNX Held | Unlocks                                              |
| ------------- | --------- | ---------------------------------------------------- |
| **Basic**     | 0         | 20 messages/day, SFW personas, watermarked images   |
| **Unlimited** | 10,000    | Unlimited SFW chat, HD images, 5 custom personas    |
| **NSFW**      | 100,000   | Everything above + Zhenox Eros + Canvas access      |
| **VIP**       | 500,000   | Early persona drops, creator revenue share, alpha   |

Tier checks run client-side (UX) and are enforced server-side on every API call via the connected wallet.

---

## Roadmap

**Phase 1 — Forge (Q2)**
Public beta, persona forge, wallet login, Zhenox Core online, bilingual UI.

**Phase 2 — Eros (Q3)**
NSFW tier live, Canvas pipeline, Eros model fine-tuning, creator royalties.

**Phase 3 — Registry (Q4)**
On-chain persona registry, transferable persona NFTs, secondary market, DAO-curated featured personas.

**Phase 4 — Sovereign (Q1 next)**
User-owned model adapters, local inference option, cross-chain persona portability, Zhenox SDK for third-party apps.

---

## Contributing

PRs welcome. Before opening one:

1. Fork and branch from `main`.
2. Run `pnpm lint && pnpm typecheck`.
3. Keep commits atomic, messages in English.
4. Never route NSFW through the Core pipeline — this is a hard CI check, not a suggestion.
5. All UI strings must ship in both `en.json` and `zh.json`.

Good first issues are tagged `good-first-issue`. Design contributions welcome via the `design/` track.

---

## License

**MIT** — do whatever you want, just don't blame us. See [LICENSE](./LICENSE).

---

## Links

- Docs — https://zhenox.com/en/docs
- App — https://zhenox.com
- Twitter — [@ZhenoxApp](https://twitter.com/ZhenoxApp)
- GitHub — [github.com/ZhenoxApp](https://github.com/ZhenoxApp)

---

*Built with 💗 for people who think chatbots should have personality — and a price tag.*
