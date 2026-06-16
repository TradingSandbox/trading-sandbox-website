# tradecli Website Framing Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Reframe and polish the tradecli website from "AI trading assistant" to "agent-native trading OS for Indian markets" while preserving the existing install/setup/doctor commands.

**Architecture:** Keep the existing static HTML + shared CSS architecture. Rewrite the homepage as a thesis-led, proof-backed command-center page, update About/Updates/wiki copy to match the current product shape, and verify with the existing build/smoke tests plus Browser screenshots on the real local preview.

**Tech Stack:** Static HTML, shared CSS tokens/components, TypeScript build scripts, VitePress wiki, Vitest smoke tests, Browser plugin for visual QA.

---

## File Structure

- Modify `index.html`: homepage narrative, hero, product model sections, safety ladder, workflow sections, keep existing install commands unchanged.
- Modify `shared/components.css`: new command-center layout classes, Herdr spawn animation classes, and responsive polish; preserve existing nav/brand edits.
- Modify `shared/nav.html`: update labels only if needed for "Platform" / "Docs" / "About"; preserve current brand asset references.
- Modify `about/index.html`: replace older market-simulation framing with mission-draft OS framing.
- Modify `updates/index.html`: replace draft-only content with real product milestones.
- Modify `build/build-pages.ts`: lift `/updates/` noindex only if the updates page is substantive after the content pass.
- Modify `wiki/index.md`: reframe docs around the OS model.
- Modify `wiki/guides/personas.md`: reframe personas as agents/modes and include Office Mode.
- Optionally create `wiki/guides/office-mode.md`: concise Office Mode doc if the personas page becomes too dense.
- Modify `build/__tests__/build-smoke.test.ts`: update sitemap expectations only if `/updates/` becomes indexable.
- Modify `build/__tests__/build-pages.test.ts`: keep manifest expectations current only if manifest indexability fields change.

## Task 1: Add Content Guardrails Before Rewriting

**Files:**
- Modify: `build/__tests__/build-smoke.test.ts`
- Modify: `build/__tests__/build-pages.test.ts` only if the updates manifest changes in Task 5.

- [ ] **Step 1: Add homepage framing smoke assertions**

In `build/__tests__/build-smoke.test.ts`, inside the existing `describe('smoke: sitemap.xml — prod build', () => { ... })` block, add this test after the `homepage has 3 JSON-LD blocks; other pages have 2` test:

```ts
  test('homepage carries the new OS framing and preserves install commands', () => {
    const html = readFileSync(join(REPO_ROOT, 'dist/index.html'), 'utf-8');
    expect(html).toContain('agent-native trading OS');
    expect(html).toContain('AITradingOffice');
    expect(html).toContain('brew install TradingSandbox/tradecli/tradecli');
    expect(html).toContain('tradecli setup');
    expect(html).toContain('tradecli doctor');
  });
```

- [ ] **Step 2: Add about/wiki/update content smoke assertions**

Still in `build/__tests__/build-smoke.test.ts`, add this test after the homepage framing test:

```ts
  test('secondary surfaces explain office memory and guardrails', () => {
    const about = readFileSync(join(REPO_ROOT, 'dist/about/index.html'), 'utf-8');
    const updates = readFileSync(join(REPO_ROOT, 'dist/updates/index.html'), 'utf-8');
    const wiki = readFileSync(join(REPO_ROOT, 'dist/wiki/index.html'), 'utf-8');

    expect(about).toContain('trader-controlled harness');
    expect(about).toContain('guardrails');
    expect(updates).toContain('Office Mode');
    expect(updates).toContain('broker gateway');
    expect(wiki).toContain('agent-native trading OS');
    expect(wiki).toContain('Office Mode');
  });
```

- [ ] **Step 3: Run tests and confirm the new assertions fail**

Run:

```bash
npm test -- build/__tests__/build-smoke.test.ts
```

Expected: FAIL because the current built content still says "AI trading assistant" and `/updates/` still contains draft-only content.

- [ ] **Step 4: Commit the failing test checkpoint**

```bash
git add build/__tests__/build-smoke.test.ts
git commit -m "test: lock website framing expectations"
```

If the user has unrelated changes in this file, inspect the diff first and include only the new test hunks.

## Task 2: Rebuild Homepage Narrative And Layout

**Files:**
- Modify: `index.html`
- Modify: `shared/components.css`

- [ ] **Step 1: Replace homepage metadata with OS framing**

In `index.html`, update the `<title>`, description, Open Graph description, and SoftwareApplication JSON-LD description to use:

```html
<title>tradecli — Agent-Native Trading OS for Indian Markets</title>
<meta name="description" content="tradecli is an agent-native trading operating system for Indian markets: local AI agents, broker/browser tools, AITradingOffice memory, and guardrailed action paths." />
<meta property="og:title" content="tradecli — Agent-Native Trading OS for Indian Markets" />
<meta property="og:description" content="Run local AI agents across brokers, charts, screeners, portfolios, and an AITradingOffice memory layer." />
```

For the JSON-LD `description`, use:

```json
"description": "Agent-native trading operating system for Indian markets: local AI agents across broker, browser, chart, portfolio, and AITradingOffice workflows with guardrailed action paths."
```

- [ ] **Step 2: Replace the hero copy while preserving the terminal surface**

In `index.html`, replace the hero badge, `h1`, supporting paragraph, and actions with:

```html
<div class="hero-badge">
  <div class="hero-badge-dot"></div>
  Built in India · Local-first · Guardrailed by design
</div>
<h1>The agent-native trading OS for Indian markets</h1>
<p>tradecli runs local AI agents across brokers, charts, screeners, portfolios, and an AITradingOffice memory layer, so your market process becomes visible, queryable, and scalable.</p>
<div class="hero-actions" style="margin-top:24px;">
  <a href="#get-started" class="btn-primary">
    ...
    Install tradecli
  </a>
  <a href="{{SITE_BASE}}wiki/" class="btn-secondary">Read the docs</a>
</div>
```

Keep the existing SVG icon inside the primary CTA. Do not change the install command block in the CTA section.

- [ ] **Step 3: Replace the personas-first section with a proof strip**

Above the existing personas/workflow content, add a `section` with class `proof-strip-section` and cards for:

```html
Local-first and auditable
Broker-aware through Groww and Zerodha/Kite
Browser-native for TradingView, Screener, and broker workflows
Multi-agent Office Mode in Herdr/tmux
AITradingOffice memory and ledgers
Guardrailed action path
```

Use short 1-line descriptions. Avoid emoji as the primary visual language; use small labels and restrained borders.

- [ ] **Step 4: Add an architecture section**

Add a section with `id="platform"` after the proof strip:

```html
<section id="platform" class="platform-section">
  <div class="container">
    <div class="section-header fade-in">
      <div class="section-label">Platform</div>
      <h2 class="section-title">One operating layer. Many agents. One memory.</h2>
      <p class="section-desc">The OS connects the tools traders already use, gives each agent a mandate, and writes durable context into AITradingOffice.</p>
    </div>
    ...
  </div>
</section>
```

The body should visually show four parts:

```text
Tools and surfaces -> tradecli OS -> Agents -> AITradingOffice memory
                         |
                      Guardrails
```

Use editable HTML/CSS boxes, not a static image.

- [ ] **Step 5: Convert the personas section into workflows**

Rename the visible section label from `Personas` to `Workflows`. Keep the existing demo machinery if it still works, but adjust the top-level copy to:

```html
<div class="section-label">Workflows</div>
<h2 class="section-title">From one operator to an AI trading office.</h2>
<p class="section-desc">Start with a single focused agent, then scale into office workflows where specialists research, monitor, review, and report back through a shared system of record.</p>
```

Update cards to emphasize:

- Learner: guided learning agent
- Investor: thesis and research agent
- Trader: setup and execution-prep agent
- Portfolio/PMS: risk, allocation, client/account context

Add one additional office workflow block if it fits cleanly:

```html
<div class="office-workflow">
  <h3>Office Mode</h3>
  <p>Launch CEO, Investor, and Trader workers in Herdr or tmux. The mailbox coordinates delegation while AITradingOffice keeps the durable record.</p>
</div>
```

- [ ] **Step 6: Add safety ladder section**

Before the install CTA, add a section with `id="guardrails"` that lists:

```html
Read-only context
Suggested action
Pre-trade risk review
Prepared order
Human approval
Config-gated execution
Audit trail
Post-trade review
```

Use ordered visual steps. Keep the existing footer disclaimer unchanged.

- [ ] **Step 7: Add CSS for the new sections**

Append focused classes to `shared/components.css`:

```css
.proof-strip-section { padding: 48px 0 88px; border-top: 1px solid var(--border); }
.proof-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 12px; }
.proof-item { border: 1px solid var(--border); background: rgba(17, 19, 24, 0.72); border-radius: 8px; padding: 18px; }
.proof-item h3 { font-size: 0.95rem; margin-bottom: 6px; }
.proof-item p { color: var(--text-secondary); font-size: 0.88rem; line-height: 1.55; }
.platform-map { display: grid; grid-template-columns: 1fr auto 1fr auto 1fr; gap: 12px; align-items: stretch; }
.platform-node { border: 1px solid var(--border-light); background: var(--bg-card); border-radius: 8px; padding: 20px; min-height: 150px; }
.platform-arrow { display: flex; align-items: center; color: var(--text-muted); font-family: 'JetBrains Mono', monospace; }
.guardrail-ladder { display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 12px; counter-reset: ladder; }
.guardrail-step { counter-increment: ladder; border: 1px solid var(--border); border-radius: 8px; padding: 16px; background: rgba(21, 24, 32, 0.72); }
.guardrail-step::before { content: counter(ladder, decimal-leading-zero); display: block; font-family: 'JetBrains Mono', monospace; color: var(--accent-light); font-size: 0.72rem; margin-bottom: 8px; }
@media (max-width: 900px) {
  .proof-grid, .guardrail-ladder { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  .platform-map { grid-template-columns: 1fr; }
  .platform-arrow { justify-content: center; }
}
@media (max-width: 640px) {
  .proof-grid, .guardrail-ladder { grid-template-columns: 1fr; }
}
```

Tune these values during Browser verification if the actual page feels cramped or too card-heavy.

- [ ] **Step 8: Run homepage build check**

Run:

```bash
npm test -- build/__tests__/build-smoke.test.ts
```

Expected: still may fail on About/Updates/Wiki assertions until later tasks, but homepage assertions should pass.

- [ ] **Step 9: Commit homepage work**

```bash
git add index.html shared/components.css
git commit -m "feat: reframe homepage as trading OS"
```

## Task 3: Update About Page Mission

**Files:**
- Modify: `about/index.html`

- [ ] **Step 1: Replace about metadata**

Use:

```html
<title>About — tradecli</title>
<meta name="description" content="Why tradecli exists: a trader-controlled harness, AITradingOffice memory, and guardrails for agent-native market workflows." />
<meta property="og:description" content="The mission behind tradecli's agent-native trading operating system." />
```

- [ ] **Step 2: Replace hero copy**

Replace the about hero body with:

```html
<div class="section-label">About Us</div>
<h1 class="section-title" style="max-width: 760px;">The missing layer between traders and their tools</h1>
<p class="section-desc" style="font-size: 1.15rem; max-width: 760px;">
  Traders already produce signal through watchlists, theses, journals, risk rules, and broker actions. tradecli turns that scattered process into a trader-controlled harness agents can reason over.
</p>
```

- [ ] **Step 3: Replace body sections**

Use three sections:

1. `The bottleneck is memory`
2. `The office is persistent`
3. `Autonomy needs guardrails`

Include these exact phrases somewhere in the page so tests pass:

```text
trader-controlled harness
AITradingOffice
guardrails
```

- [ ] **Step 4: Run build-smoke partial check**

Run:

```bash
npm test -- build/__tests__/build-smoke.test.ts
```

Expected: About assertions pass; Updates/Wiki may still fail.

- [ ] **Step 5: Commit about page**

```bash
git add about/index.html
git commit -m "docs: update about mission framing"
```

## Task 4: Update Wiki Framing And Modes

**Files:**
- Modify: `wiki/index.md`
- Modify: `wiki/guides/personas.md`
- Create: `wiki/guides/office-mode.md` if needed

- [ ] **Step 1: Update wiki index intro**

In `wiki/index.md`, replace the first description with:

```md
Documentation for `tradecli` — an agent-native trading OS for Indian markets.
```

Update "What is tradecli?" to explain:

```md
`tradecli` is a local command layer for market workflows. It runs AI agents across broker APIs, browser automation, TradingView, Screener.in, channels, and AITradingOffice, the local system of record for theses, trades, journals, reviews, clients, employees, and ledgers.
```

- [ ] **Step 2: Add docs entries**

Add guide links or stubs to the Explore section:

```md
#### [Office Mode](/guides/office-mode)
- Herdr/tmux office launch
- CEO, Investor, Trader workers
- AITradingOffice memory
- Guardrails and review flow
```

If `office-mode.md` is not created, point to Personas & Modes and include the Office Mode bullets there.

- [ ] **Step 3: Update personas page**

In `wiki/guides/personas.md`, change the lead to:

```md
`tradecli` modes are agents running on the same local operating layer. Some are single-operator modes; Office Mode starts multiple workers and coordinates them through a shared mailbox and AITradingOffice state.
```

Update the table to include:

```md
| Run a multi-agent desk with CEO, Investor, and Trader workers | **Office Mode** |
```

- [ ] **Step 4: Create office-mode guide if the page needs room**

Create `wiki/guides/office-mode.md` with:

```md
---
title: Office Mode
description: Run multi-agent tradecli offices in Herdr or tmux with AITradingOffice-backed memory.
outline: 2
---

# Office Mode

Office Mode starts a small trading desk instead of one assistant. A CEO agent delegates work to specialist workers such as Investor and Trader, while the mailbox coordinates tasks and AITradingOffice keeps durable state.

## Launch

```bash
tradecli office --tmux
tradecli office --herdr
```

## What it uses

- Herdr or tmux for visible panes
- AITradingOffice for roster, theses, trades, ledgers, and reviews
- Broker gateway for shared broker-side context
- Guardrails for action review and execution gates

## Safety

Execution is not automatic by default. Portfolio-updating tools and broker actions remain gated by configuration, explicit approval, and broker/tool confirmation.
```

- [ ] **Step 5: Run wiki build**

Run:

```bash
npm run build:wiki
```

Expected: VitePress builds `dist/wiki/` without broken frontmatter.

- [ ] **Step 6: Commit wiki updates**

```bash
git add wiki/index.md wiki/guides/personas.md wiki/guides/office-mode.md
git commit -m "docs: reframe wiki around office OS"
```

If `office-mode.md` was not created, omit it from `git add`.

## Task 5: Replace Updates Page With Real Milestones

**Files:**
- Modify: `updates/index.html`
- Modify: `build/build-pages.ts`
- Modify: `build/__tests__/build-smoke.test.ts` if indexability changes expected sitemap count.

- [ ] **Step 1: Rewrite updates page content**

Replace the placeholder cards in `updates/index.html` with real milestones:

```html
<h3>v0.6.x — Office Mode becomes the visible desk</h3>
<p>tradecli can launch CEO, Investor, and Trader workers in Herdr or tmux, with mailbox coordination and AITradingOffice-backed state.</p>

<h3>AITradingOffice joins the local stack</h3>
<p>The office service stores theses, forward tests, ledgers, employees, clients, PMS accounts, and hedge-fund records through local APIs.</p>

<h3>Broker gateway and sidecar ownership</h3>
<p>Broker tools now run through a profile-scoped local gateway so office panes share broker context instead of racing separate sessions.</p>

<h3>Setup, doctor, and upgrade hygiene</h3>
<p>The Homebrew stack manages tradecli and AITradingOffice together, while setup and doctor checks keep browser, broker, model, port, and version health visible.</p>
```

Keep wording factual and tied to README-backed behavior.

- [ ] **Step 2: Decide whether to index updates**

If the rewritten page has at least the four real milestones above and no bracketed draft text, change the manifest entry in `build/build-pages.ts` from:

```ts
{ source: 'updates/index.html', output: 'updates/index.html', changefreq: 'weekly', priority: 0.8, indexable: false, robots: 'noindex, follow' },
```

to:

```ts
{ source: 'updates/index.html', output: 'updates/index.html', changefreq: 'weekly', priority: 0.8, indexable: true },
```

- [ ] **Step 3: Update sitemap smoke expectations if updates is indexable**

In `build/__tests__/build-smoke.test.ts`, change:

```ts
expect(xml).not.toMatch(/updates/);
```

to:

```ts
expect(xml).toContain('<loc>https://tradecli.in/updates/</loc>');
```

Change:

```ts
expect(urlBlocks.length).toBe(2);
```

to:

```ts
expect(urlBlocks.length).toBe(3);
```

Keep 404 excluded.

- [ ] **Step 4: Run page tests**

Run:

```bash
npm test -- build/__tests__/build-pages.test.ts build/__tests__/build-smoke.test.ts
```

Expected: tests pass if all content assertions and sitemap counts match.

- [ ] **Step 5: Commit updates page**

```bash
git add updates/index.html build/build-pages.ts build/__tests__/build-smoke.test.ts
git commit -m "docs: publish real product updates"
```

If `build/build-pages.ts` and tests were not changed because the page remains noindexed, omit them from `git add`.

## Task 6: Navigation And Responsive Polish

**Files:**
- Modify: `shared/nav.html`
- Modify: `shared/components.css`

- [ ] **Step 1: Update nav labels only if the homepage sections exist**

If `index.html` now has `#platform`, change desktop and mobile nav links from:

```html
<li><a href="{{SITE_BASE}}#personas">Personas</a></li>
<li><a href="{{SITE_BASE}}#features">Features</a></li>
```

to:

```html
<li><a href="{{SITE_BASE}}#platform">Platform</a></li>
<li><a href="{{SITE_BASE}}#workflows">Workflows</a></li>
```

Make the same change in `.nav-mobile`. Preserve logo markup and brand assets.

- [ ] **Step 2: Remove stale homepage sections only when replaced**

If the old `#features` section is redundant after the proof and architecture sections, remove it from `index.html` rather than keeping duplicate claims. Keep `#integrations` only if it still helps the platform proof.

- [ ] **Step 3: Run build**

Run:

```bash
npm run build
```

Expected: build succeeds, marketing pages and wiki render into `dist/`.

- [ ] **Step 4: Commit navigation polish**

```bash
git add shared/nav.html shared/components.css index.html
git commit -m "style: polish website navigation and layout"
```

## Task 7: Herdr Multi-Agent Spawn Animation

**Files:**
- Modify: `index.html`
- Modify: `shared/components.css`

- [ ] **Step 1: Replace or reduce wonky persona animation emphasis**

Decide whether the new animation lives in the `#platform` section or the `#workflows` section. Prefer `#platform` if the architecture section needs concrete proof; prefer `#workflows` if the page already explains architecture clearly.

Do not remove the existing install/setup/doctor CTA.

- [ ] **Step 2: Add stable animation markup**

Add this editable product animation block near the architecture/workflow proof:

```html
<div class="herdr-demo" aria-label="Office Mode spawning agents in Herdr">
  <div class="herdr-demo-header">
    <span>tradecli office --herdr</span>
    <button class="herdr-demo-replay" type="button" aria-label="Replay Office Mode animation">Replay</button>
  </div>
  <div class="herdr-demo-stage">
    <div class="herdr-pane herdr-pane-ceo">
      <span class="pane-role">CEO</span>
      <strong>Open market brief</strong>
      <p>Delegate research, setup, and risk checks.</p>
    </div>
    <div class="herdr-mailbox">
      <span class="mail-chip mail-chip-1">thesis task</span>
      <span class="mail-chip mail-chip-2">chart setup</span>
      <span class="mail-chip mail-chip-3">risk review</span>
    </div>
    <div class="herdr-pane herdr-pane-investor">
      <span class="pane-role">Investor</span>
      <strong>Thesis update</strong>
      <p>AITradingOffice record ready.</p>
    </div>
    <div class="herdr-pane herdr-pane-trader">
      <span class="pane-role">Trader</span>
      <strong>Setup checked</strong>
      <p>Prepared action needs approval.</p>
    </div>
    <div class="herdr-pane herdr-pane-pms">
      <span class="pane-role">PMS</span>
      <strong>Exposure reviewed</strong>
      <p>Guardrail checkpoint passed.</p>
    </div>
    <div class="herdr-office-log">
      <span>AITradingOffice</span>
      <strong>thesis · trade · review · audit trail</strong>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Add stable animation CSS**

Append CSS that uses fixed slots and does not resize during animation:

```css
.herdr-demo {
  border: 1px solid var(--border-light);
  border-radius: 8px;
  background: #07090d;
  overflow: hidden;
  box-shadow: 0 24px 80px rgba(0, 0, 0, 0.35);
}
.herdr-demo-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  min-height: 44px;
  padding: 10px 14px;
  border-bottom: 1px solid var(--border);
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.78rem;
  color: var(--text-secondary);
}
.herdr-demo-replay {
  border: 1px solid var(--border);
  background: var(--bg-card);
  color: var(--text-secondary);
  border-radius: 6px;
  padding: 6px 10px;
  font: inherit;
  cursor: pointer;
}
.herdr-demo-stage {
  position: relative;
  min-height: 420px;
  aspect-ratio: 16 / 9;
  padding: 18px;
}
.herdr-pane,
.herdr-office-log,
.herdr-mailbox {
  position: absolute;
  border: 1px solid var(--border);
  border-radius: 8px;
  background: rgba(17, 19, 24, 0.94);
}
.herdr-pane {
  width: min(31%, 260px);
  min-height: 112px;
  padding: 14px;
}
.herdr-pane strong,
.herdr-office-log strong {
  display: block;
  font-size: 0.95rem;
  line-height: 1.3;
}
.herdr-pane p {
  margin-top: 8px;
  color: var(--text-secondary);
  font-size: 0.8rem;
  line-height: 1.45;
}
.pane-role {
  display: block;
  font-family: 'JetBrains Mono', monospace;
  color: var(--accent-light);
  font-size: 0.72rem;
  margin-bottom: 8px;
}
.herdr-pane-ceo { left: 18px; top: 18px; }
.herdr-pane-investor { right: 18px; top: 18px; animation: pane-spawn 14s ease-in-out infinite; animation-delay: 1.6s; }
.herdr-pane-trader { right: 18px; top: 150px; animation: pane-spawn 14s ease-in-out infinite; animation-delay: 3.2s; }
.herdr-pane-pms { right: 18px; top: 282px; animation: pane-spawn 14s ease-in-out infinite; animation-delay: 4.8s; }
.herdr-mailbox {
  left: 38%;
  top: 112px;
  width: 22%;
  min-height: 178px;
  padding: 12px;
}
.mail-chip {
  display: block;
  border: 1px solid var(--border);
  border-radius: 999px;
  padding: 7px 9px;
  margin-bottom: 10px;
  color: var(--text-secondary);
  font-size: 0.74rem;
  font-family: 'JetBrains Mono', monospace;
  animation: mail-pulse 14s ease-in-out infinite;
}
.mail-chip-1 { animation-delay: 1.2s; }
.mail-chip-2 { animation-delay: 2.8s; }
.mail-chip-3 { animation-delay: 4.4s; }
.herdr-office-log {
  left: 18px;
  right: 18px;
  bottom: 18px;
  min-height: 72px;
  padding: 14px;
}
.herdr-office-log span {
  display: block;
  font-family: 'JetBrains Mono', monospace;
  color: var(--green);
  font-size: 0.72rem;
  margin-bottom: 8px;
}
@keyframes pane-spawn {
  0%, 10% { opacity: 0.2; transform: translateX(12px); }
  18%, 82% { opacity: 1; transform: translateX(0); }
  100% { opacity: 0.95; transform: translateX(0); }
}
@keyframes mail-pulse {
  0%, 12% { border-color: var(--border); color: var(--text-muted); }
  18%, 34% { border-color: var(--accent-light); color: var(--text-primary); }
  48%, 100% { border-color: var(--border); color: var(--text-secondary); }
}
@media (prefers-reduced-motion: reduce) {
  .herdr-pane,
  .mail-chip {
    animation: none;
  }
}
@media (max-width: 760px) {
  .herdr-demo-stage {
    aspect-ratio: auto;
    min-height: 640px;
  }
  .herdr-pane,
  .herdr-mailbox,
  .herdr-office-log {
    position: static;
    width: 100%;
    margin-bottom: 10px;
  }
}
```

Tune after Browser screenshots. The CSS above is the starting point, not a substitute for visual QA.

- [ ] **Step 4: Add replay behavior only if the button is kept**

If the replay button remains visible, add a tiny script near the homepage scripts:

```js
document.querySelectorAll('.herdr-demo-replay').forEach((button) => {
  button.addEventListener('click', () => {
    const demo = button.closest('.herdr-demo');
    if (!demo) return;
    demo.classList.remove('replay');
    void demo.offsetWidth;
    demo.classList.add('replay');
  });
});
```

If this class is used, scope animation selectors with `.herdr-demo.replay` or keep the button out of the first implementation.

- [ ] **Step 5: Run build**

Run:

```bash
npm run build
```

Expected: build succeeds.

- [ ] **Step 6: Commit animation**

```bash
git add index.html shared/components.css
git commit -m "feat: add Herdr office mode animation"
```

## Task 8: Browser Visual QA And Final Iteration

**Files:**
- Modify: any page/CSS files found by visual QA.

- [ ] **Step 1: Start local preview**

Run:

```bash
npm run preview
```

Expected: local server prints a URL on port 3000 unless occupied. If occupied, rerun with a different serve port:

```bash
npx serve dist/ -p 3001 --no-clipboard
```

- [ ] **Step 2: Open real preview in Browser plugin**

Use Browser to open:

```text
http://localhost:3000/
```

or the alternate port.

- [ ] **Step 3: Inspect desktop homepage**

Check:

- first viewport shows the OS thesis, proof, and CTA without crowding
- terminal/system proof reads as product evidence
- no text overlaps
- no button text clips
- palette is restrained and not dominated by purple/blue gradients

- [ ] **Step 4: Inspect mobile homepage**

Use a mobile viewport. Check:

- hero headline wraps cleanly
- proof cards stack without oversized gaps
- platform map becomes readable in one column
- mobile nav labels fit
- install commands remain visible and unchanged

- [ ] **Step 5: Inspect secondary pages**

Open:

```text
http://localhost:3000/about/
http://localhost:3000/updates/
http://localhost:3000/wiki/
http://localhost:3000/wiki/guides/personas
```

Check that copy is aligned with the OS/product model and no page looks like a half-updated older site.

- [ ] **Step 6: Patch visual issues**

Use `apply_patch` for any CSS or HTML changes. Keep fixes scoped to visible issues found in Browser.

- [ ] **Step 7: Rebuild and retest**

Run:

```bash
npm test
npm run build
```

Expected: all tests pass and build succeeds.

- [ ] **Step 8: Commit visual QA fixes**

```bash
git add index.html about/index.html updates/index.html wiki/index.md wiki/guides/personas.md wiki/guides/office-mode.md shared/components.css shared/nav.html build/build-pages.ts build/__tests__/build-smoke.test.ts
git commit -m "style: finish website OS framing polish"
```

Only add files that actually changed.

## Task 9: Final Review

**Files:**
- Review all changed files.

- [ ] **Step 1: Inspect final diff**

Run:

```bash
git diff --stat HEAD~6..HEAD
git diff HEAD~6..HEAD -- index.html about/index.html updates/index.html wiki/index.md wiki/guides/personas.md shared/components.css shared/nav.html
```

Expected: changes are limited to the website framing/polish scope.

- [ ] **Step 2: Confirm unrelated changes were not reverted**

Run:

```bash
git status --short
```

Expected: any pre-existing user changes outside this work remain as they were unless deliberately integrated.

- [ ] **Step 3: Prepare final summary**

Summarize:

- pages updated
- major framing change
- install/setup/doctor preserved
- tests/build run
- Browser pages checked
