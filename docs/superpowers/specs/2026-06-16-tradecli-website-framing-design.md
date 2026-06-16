# tradecli Website Framing And Polish Design

Date: 2026-06-16
Status: approved for implementation planning

## Goal

Reframe `tradecli.in` around the current product shape: an agent-native trading operating system for Indian markets, not a generic AI trading assistant. The site should satisfy two audiences at once:

- serious traders/operators who may install and try it now
- investors/founders who need to understand the company thesis

The homepage should be thesis-led and proof-backed: ambitious enough to explain the operating-system vision, but grounded enough that visitors can see what already ships today.

## Mission Anchor

Use the mission draft in `tradecli-writing/drafts/2026-06-agent-native-trading-operating-system.md` as the positioning source. The core message:

`tradecli` is an agent-native trading operating system: a local harness that plugs into the tools traders already use, learns how they trade, and runs agents that execute within explicit rules, from one operator to a full desk.

The public site should avoid implying unsupervised live trading is broadly available by default. Autonomy must be presented as earned through guardrails: read-only context, suggested action, risk review, prepared order, human approval, config-gated execution, audit trail, and post-trade review.

## Current Gap

The current website and wiki mostly say "AI trading assistant for Indian markets" and organize around four personas. That undersells the present product:

- `tradecli` now has Office Mode in Herdr/tmux.
- AITradingOffice is a local system of record for theses, trades, forward tests, employees, clients, PMS, and hedge fund state.
- Broker gateway, browser automation, TradingView, Screener, Telegram, WhatsApp, and local sidecars make the product feel like infrastructure, not a chatbot.
- Release and upgrade flow has matured through the `0.6.x` line.

The site needs to make those parts legible as one platform.

## Product Model

Present `tradecli` as one platform with four layers:

1. **Operating layer:** local command layer that connects broker APIs, browser automation, TradingView, Screener, schedules, channels, and sidecars.
2. **Agents:** Learner, Investor, Trader, PMS, and Hedge Fund / Office workers that run on the OS with distinct mandates.
3. **Memory:** AITradingOffice as the system of record for theses, trades, journals, reviews, clients, employees, ledgers, and outcomes.
4. **Guardrails:** visible local control, human approval, config-gated execution, and an audit trail.

This replaces the older mental model of "four personas" as the main product. Personas remain important, but they are agents running on the operating layer.

## Homepage Design

### 1. Hero

Lead with:

> The agent-native trading OS for Indian markets.

Support with one short paragraph:

> `tradecli` runs local AI agents across brokers, charts, screeners, portfolios, and an AITradingOffice memory layer, so your trading process becomes visible, queryable, and scalable.

Primary CTA: "Install tradecli"

Secondary CTA: "Read the docs" or "See how it works"

The hero should not be a marketing split-card layout. It should feel like a serious command center: restrained typography, visible terminal/system proof, and no decorative filler.

### 2. Proof Strip

Immediately beneath the hero, show concrete proof points:

- Local-first and auditable
- Broker-aware through Groww and Zerodha/Kite
- Browser-native for TradingView, Screener, and broker workflows
- Multi-agent Office Mode in Herdr/tmux
- AITradingOffice memory and ledgers
- Guardrailed action path

### 3. Architecture Section

Explain the platform as:

```text
Tools and surfaces -> tradecli OS -> Agents -> AITradingOffice memory
                         |
                      Guardrails
```

This should be visual in the site, using HTML/CSS rather than a static diagram where practical, so it stays responsive and editable.

### 4. Workflow Section

Show the product through workflows, not feature cards:

- **Solo operator:** ask, research, inspect charts, prepare action.
- **Investor:** turn discovery into thesis, review, and forward tests.
- **PMS/family office:** manage client/account context, holdings, ledgers, risk, and review.
- **Hedge fund desk:** spawn CEO, Investor, Trader, and workers across panes.

Each workflow should identify the agent, the tools it uses, what gets remembered, and where guardrails apply.

### 5. Safety Ladder

Make the autonomy path explicit:

1. Read-only context
2. Suggested action
3. Pre-trade risk review
4. Prepared order
5. Human approval
6. Config-gated execution
7. Audit trail
8. Post-trade review

This section is important for trust, regulatory tone, and product credibility.

### 6. Install CTA

Keep the install path concrete:

```bash
brew install TradingSandbox/tradecli/tradecli
tradecli setup
tradecli doctor
tradecli
```

Link into Quick Start and relevant docs. The CTA should make the product feel shippable today.

## Secondary Pages

### Wiki Index

Update the wiki index from "terminal-based AI trading assistant" to "agent-native trading OS." Add entries for:

- Office Mode
- AITradingOffice
- Broker gateway
- Guardrails and portfolio update safety
- Channels

Existing quick start material stays practical and install-focused.

### Personas And Modes

Reframe personas as agents/modes running on the OS. Add Office Mode, Hedge Fund, and PMS-oriented context where supported by the current product and docs.

### About

Replace the older "market simulation" vision with the mission-draft framing:

- traders already produce signal through watchlists, theses, journals, and actions
- the bottleneck is attention, memory, and continuity
- `tradecli` captures that signal into a trader-controlled harness
- AITradingOffice makes the work durable
- guardrails make future autonomy responsible

### Updates

Replace the current draft-only content with real release/product milestones. Include:

- `0.6.x` line
- Office Mode in Herdr/tmux
- AITradingOffice local service integration
- broker gateway
- setup/doctor/upgrade hygiene
- Telegram bridge and channel direction where appropriate

If the content is real enough, lift the current noindex setting for `/updates/`.

## Visual Direction

Use a serious financial command-center feel:

- restrained palette, not one-note neon or generic SaaS gradients
- strong typographic hierarchy
- dense but readable sections
- terminal/system surfaces as proof, not decoration
- fewer generic cards, more structured workflow and architecture blocks
- mobile layouts that preserve hierarchy without hiding the product model

Avoid overpromising with words like "autonomous trading" unless the guardrail context is adjacent.

## Content Rules

- Say "agent-native trading OS" at the top level.
- Use "AI assistant" only as an approachable secondary phrase.
- Keep India/NSE/BSE as the initial wedge.
- Mention third-party platforms accurately and preserve affiliation disclaimers.
- Do not imply financial advice.
- Do not imply live execution is automatic by default.
- Prefer "prepare", "review", "approve", and "execute when explicitly enabled" for action language.

## Implementation Units

1. Homepage copy and section restructuring in `index.html`.
2. Shared styling updates in `shared/components.css` and `shared/tokens.css` if needed.
3. Navigation label updates in `shared/nav.html` if the information architecture changes.
4. About page copy update in `about/index.html`.
5. Updates page replacement in `updates/index.html` and manifest indexability decision in `build/build-pages.ts`.
6. Wiki index/persona docs updates under `wiki/`.
7. Build and browser verification.

## Error Handling And Risk

- Existing user changes are present in `assets/favicon.svg`, `shared/components.css`, `shared/nav.html`, and `assets/brand/`; implementation must preserve and build on those changes.
- The production/preview bridge is unusual. Work should happen in the current website repo and should not touch deployment workflow files unless explicitly required.
- If `/updates/` remains partly editorial or incomplete, keep it noindexed.
- If any product capability is uncertain, phrase it as "supports", "can", or "is evolving" based on README-backed facts, not as a promise about future behavior.

## Verification

Run:

```bash
npm test
npm run build
```

Then serve the site and inspect real pages in Browser:

```bash
npm run preview
```

Check at least:

- desktop homepage first viewport
- desktop full homepage scan
- mobile homepage first viewport
- mobile navigation
- About page
- Updates page
- Wiki index

Verification criteria:

- no broken build, JSON-LD, sitemap, or smoke tests
- hero text fits on mobile and desktop
- CTAs are visible and useful
- sections read in the intended product model order
- no incoherent overlap or text clipping
- color palette feels restrained and professional
- disclaimers remain present
- no unsupported claims about financial advice or automatic execution

## Out Of Scope

- New backend/product behavior
- New analytics infrastructure
- New deployment workflow behavior
- Full docs rewrite beyond the pages needed to close the framing gap
- New generated bitmap imagery unless a later design pass explicitly calls for it
