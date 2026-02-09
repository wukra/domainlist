# CLAUDE.md

## Project Overview

This repository maintains curated domain lists for **Clash** proxy rule configuration. Each `.list` file defines routing rules that determine how network traffic is directed (proxied, direct, etc.) based on domains, IPs, ports, or process names.

This is a **data-only repository** — no build system, no tests, no CI/CD, and no programming language dependencies.

## Repository Structure

```
domainlist/
├── CLAUDE.md          # This file
├── README.md          # Brief project description
└── list/              # All domain list files
    ├── ai.list        # AI/ML service domains (OpenAI, Anthropic, Google, etc.)
    ├── crypto.list    # Cryptocurrency ecosystem (exchanges, DeFi, wallets, etc.)
    ├── direct.list    # Rules for direct (non-proxied) routing
    └── tailscale.list # Tailscale network routing rules
```

## Clash Rule Format

Each line in a `.list` file is a Clash routing rule. Supported rule types used in this repo:

| Rule Type | Example | Description |
|---|---|---|
| `DOMAIN-SUFFIX` | `DOMAIN-SUFFIX,openai.com` | Matches domain and all subdomains |
| `DOMAIN` | `DOMAIN,ai.com` | Exact domain match only |
| `DOMAIN-KEYWORD` | `DOMAIN-KEYWORD,openai` | Matches if domain contains keyword |
| `IP-CIDR` | `IP-CIDR,100.64.0.0/10` | Matches IP address range |
| `DST-PORT` | `DST-PORT,53` | Matches destination port |
| `PROCESS-NAME` | `PROCESS-NAME,Binance.exe` | Matches originating process |

## Conventions for Editing List Files

- **One rule per line.** No trailing commas or semicolons.
- **Comments** use `#` prefix. Section headers use `# > SectionName` format (e.g., `# > Exchange`, `# > NFT`).
- **Inline comments** explaining additions use `# Add <description>` on the line above (e.g., `# Add ai.com`).
- **Blank lines** separate logical sections for readability.
- **Prefer `DOMAIN-SUFFIX`** for most domain rules — it covers the domain and all subdomains.
- **Use `DOMAIN`** only when an exact match is needed (no subdomain matching).
- **Use `DOMAIN-KEYWORD`** sparingly — it's broad and can cause unintended matches.
- **Group related domains** together under a section header comment.
- **Avoid duplicate entries.** Check for existing rules before adding new ones (some duplicates currently exist in the files).

## File Descriptions

- **ai.list** — Domains for AI services: OpenAI/ChatGPT, Google Gemini, Anthropic/Claude, Microsoft, Cursor, GitHub Copilot, OpenRouter, and supporting services (Auth0, Stripe, hCaptcha, Sentry).
- **crypto.list** — Organized by category with section headers: exchanges (Binance, Huobi, OKX, Coinbase, etc.), blockchain platforms (Ethereum, EOS, Solana), DeFi (Uniswap, Aave, Compound), analysis tools (CoinGecko, TradingView, DeFi Llama), NFT marketplaces, wallets, mining pools, and IPFS.
- **direct.list** — Rules for traffic that should bypass the proxy (local/internal services, DNS port 53).
- **tailscale.list** — Tailscale CGNAT range (`100.64.0.0/10`) and related domains.

## Workflow

There is no build, lint, or test step. Changes consist of adding, removing, or updating domain rules in the `.list` files. Validate changes by:

1. Ensuring correct Clash rule syntax (`TYPE,value` format).
2. Checking for duplicate entries within the same file.
3. Placing new rules in the appropriate file and section.
