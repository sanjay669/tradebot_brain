# trader-brain 🧠

An [Obsidian](https://obsidian.md) vault — the "brain" for the **upstox-trading-bot**
(sibling repo `../upstox-trading-bot`, GitHub: `sanjay669/TraderBot`). It holds the
rules, strategy notes, architecture docs, operational runbooks, compliance notes,
and the trading-review journal that the code itself deliberately does **not** carry.

## How to use it
1. Install Obsidian (free).
2. **Open folder as vault** → select this `trader-brain` folder.
3. Start at [[Home]] — it's the map of content (MOC) linking everything.

## Why a separate vault
Code changes for logic; the brain changes for *decisions and knowledge*. Keeping
them in separate repos means the review loop, rules, and post-mortems have a stable
home that isn't churned by code commits — and the code repo stays lean.

## Layout
- `Rules/` — risk, trading, and the go-live checklist (the hard gates).
- `Compliance/` — SEBI algo-trading rules that constrain what's allowed.
- `Strategies/` — one note per strategy/agent, plus a template.
- `Architecture/` — how the bot is built and how data flows.
- `Operations/` — runbooks: morning start, token refresh, Docker, incidents.
- `Reference/` — Upstox API notes, config reference, glossary.
- `Journal/` — daily review notes (the "learning from mistakes" loop).
- `Templates/` — fill-in templates for reviews, post-mortems, new strategies.

> This vault is documentation, not investment advice. See the code repo's disclaimer.
