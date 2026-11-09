# Build Brand Demo Screens

Build a complete set of self-contained HTML presentation screens for a brand × Salesforce demo. These screens are designed for screen capture into demo videos — not for browser navigation. Each file is a standalone full-viewport scene with keyboard/click state-machine navigation.

---

## Quick-start checklist

Before writing any HTML, collect:

1. **Brand assets**
   - `brand-logo.png` — client logo (used top-left on every screen alongside `salesforce-cloud.png`)
   - Background video(s): looping `.mp4` — aerial/product/lifestyle footage (~10–30s loops, muted)
   - Persona photos: 3–4 headshots as `persona-name.jpg` (used in profile cards and talking-head screens)
   - Product images: 2–3 hero product shots as PNGs (used in product pages and profile card product tiles)
   - `iphone-frame.png` if building a mobile notification overlay

2. **Brand variables** (fill these in before generating any screen)
   ```
   BRAND_NAME: e.g. "Nike"
   BRAND_PRIMARY: e.g. "#111111"   ← main brand color
   BRAND_ACCENT: e.g. "#E5161A"    ← highlight/CTA color
   SF_BLUE: "#00A1E0"              ← always Salesforce blue
   KNOWN_GREEN: "#10B981"          ← always green for "known customer"
   SIGNAL_AMBER: "#F59E0B"         ← always amber for signals/alerts
   FONT: "Inter"                   ← always Inter from Google Fonts
   ```

3. **Personas** (2–3 are enough for a full deck)
   - Name, role/segment (e.g. "Loyal Owner", "New Customer")
   - 2–3 data signals per persona (e.g. "Owns V10 since 2022", "Searched 3× this week")
   - 2–3 product affinities (for profile card product tiles)

4. **Salesforce cloud names** for the bottom strip — the four pillars shown on every screen:
   - Typically: `B2C Commerce`, `Storefront Next`, `Agentforce`, `Data Cloud`
   - Adjust to what's being demo'd

---

## Shared boilerplate (copy into every screen)

### HTML head
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Screen name]</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
  <style>
    /* ── Reset ── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { width: 100%; height: 100%; overflow: hidden; background: #0a0a0a; font-family: 'Inter', sans-serif; }
  </style>
</head>
```

### CSS design tokens (paste into every `<style>` block)
```css
:root {
  --black:   #0a0a0a;
  --white:   #ffffff;
  --sf-blue: #00A1E0;
  --known:   #10B981;
  --signal:  #F59E0B;
  --danger:  #f87171;
  --purple:  #8B5CF6;
  --brand:   /* BRAND_ACCENT */;
}
```

### Background video layer (paste into every `<body>`)
```html
<video class="bg-video" autoplay muted loop playsinline>
  <source src="your-bg.mp4" type="video/mp4">
</video>
<div class="bg-overlay"></div>
```
```css
.bg-video {
  position: fixed; inset: 0; width: 100%; height: 100%;
  object-fit: cover; z-index: 0;
}
.bg-overlay {
  position: fixed; inset: 0; z-index: 1;
  background: rgba(0,0,0,0.72);   /* adjust darkness 0.55–0.82 per screen mood */
}
```

### Dual logo (top-left, every screen)
```html
<div class="logo-bar">
  <img src="brand-logo.png" alt="Brand" class="logo-brand">
  <div class="logo-sep"></div>
  <img src="salesforce-cloud.png" alt="Salesforce" class="logo-sf">
</div>
```
```css
.logo-bar {
  position: fixed; top: 28px; left: 36px; z-index: 10;
  display: flex; align-items: center; gap: 16px;
}
.logo-brand { height: 28px; filter: brightness(0) invert(1); }
.logo-sf    { height: 22px; }
.logo-sep   { width: 1px; height: 28px; background: rgba(255,255,255,0.3); }
```

### Bottom strip (every screen)
```html
<div class="bottom-strip">
  <span>B2C Commerce</span>
  <span class="sep">·</span>
  <span>Storefront Next</span>
  <span class="sep">·</span>
  <span>Agentforce</span>
  <span class="sep">·</span>
  <span>Data Cloud</span>
</div>
```
```css
.bottom-strip {
  position: fixed; bottom: 0; left: 0; right: 0; height: 48px; z-index: 30;
  display: flex; align-items: center; justify-content: center; gap: 20px;
  background: rgba(8,8,8,0.82); backdrop-filter: blur(20px);
  border-top: 1px solid rgba(255,255,255,0.08);
  font-size: 0.65rem; font-weight: 600; letter-spacing: 0.16em;
  text-transform: uppercase; color: rgba(255,255,255,0.5);
}
.bottom-strip .sep { color: rgba(255,255,255,0.2); }
```

### Glass card pattern (reused everywhere)
```css
.glass-card {
  background: rgba(8,8,8,0.82);
  backdrop-filter: blur(24px);
  -webkit-backdrop-filter: blur(24px);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 20px;
}
```

### State-machine navigation (paste into every `<script>`)
```js
let state = 0;

function goState(n) {
  // hide all .state-X elements, show .state-N
  document.querySelectorAll('[class*="state-"]').forEach(el => {
    el.classList.remove('show');
  });
  document.querySelectorAll(`.state-${n}`).forEach(el => el.classList.add('show'));
  state = n;
}

document.addEventListener('keydown', e => {
  if (e.key === 'ArrowRight' || e.key === ' ') goState(Math.min(state + 1, MAX_STATE));
  if (e.key === 'ArrowLeft')                   goState(Math.max(state - 1, 0));
});
document.addEventListener('click', () => goState(Math.min(state + 1, MAX_STATE)));
```

### Standard animation keyframes
```css
@keyframes fadeUp   { from { opacity:0; transform:translateY(20px); } to { opacity:1; transform:translateY(0); } }
@keyframes slideIn  { from { opacity:0; transform:translateX(-24px); } to { opacity:1; transform:translateX(0); } }
@keyframes fadeIn   { from { opacity:0; } to { opacity:1; } }
@keyframes pop      { 0%{transform:scale(0.85);opacity:0;} 60%{transform:scale(1.05);} 100%{transform:scale(1);opacity:1;} }
@keyframes pulse    { 0%,100%{opacity:1;} 50%{opacity:0.4;} }
@keyframes notif-drop { from{transform:translateY(-60px);opacity:0;} to{transform:translateY(0);opacity:1;} }
```

---

## Screen type catalog

### 1 · Splash / Intro
*One scene. Logo lockup centered on video background. Used as deck opener or partnership slide.*

```html
<div class="intro-card glass-card">
  <div class="lockup">
    <img src="brand-logo.png" class="intro-logo-brand">
    <span class="intro-x">×</span>
    <img src="salesforce-cloud.png" class="intro-logo-sf">
  </div>
  <p class="intro-tagline">Redefining retail with AI</p>
</div>
```
```css
.intro-card {
  position: fixed; top: 50%; left: 50%; transform: translate(-50%,-50%);
  padding: 48px 72px; text-align: center; z-index: 20;
  animation: fadeIn 0.9s ease forwards;
}
.lockup { display: flex; align-items: center; gap: 28px; }
.intro-logo-brand { height: 36px; filter: brightness(0) invert(1); }
.intro-logo-sf    { height: 30px; }
.intro-x { font-size: 2rem; font-weight: 300; color: rgba(255,255,255,0.4); }
.intro-tagline {
  margin-top: 18px; font-size: 0.8rem; font-weight: 500;
  letter-spacing: 0.2em; text-transform: uppercase; color: rgba(255,255,255,0.55);
}
```
**Customise:** swap tagline, adjust padding for logo sizes.

---

### 2 · Talking Head (Speaker intro)
*One scene. Floating video centered with a lower-third name card. Used to introduce a named speaker.*

```html
<div class="float-stack">
  <div class="float-video-wrap">
    <video autoplay muted loop><source src="speaker.mp4" type="video/mp4"></video>
  </div>
  <div class="lower-third glass-card">
    <div class="lt-text">
      <span class="lt-name">Jane Smith</span>
      <span class="lt-title">VP Digital Commerce · Brand Inc</span>
    </div>
    <div class="lt-logos">
      <img src="brand-logo.png"><div class="lt-sep"></div><img src="salesforce-cloud.png">
    </div>
  </div>
</div>
```
```css
.float-stack {
  position: fixed; top: 50%; left: 50%; transform: translate(-50%,-50%);
  display: flex; flex-direction: column; align-items: center; gap: 0; z-index: 20;
}
.float-video-wrap {
  width: 860px; height: 540px; border-radius: 16px; overflow: hidden;
  box-shadow: 0 32px 80px rgba(0,0,0,0.7);
}
.float-video-wrap video { width: 100%; height: 100%; object-fit: cover; }
.lower-third {
  width: 860px; margin-top: -1px; border-radius: 0 0 16px 16px;
  padding: 18px 28px; display: flex; align-items: center; justify-content: space-between;
}
.lt-name  { display: block; font-size: 1.1rem; font-weight: 700; color: #fff; }
.lt-title { display: block; font-size: 0.75rem; font-weight: 400; color: rgba(255,255,255,0.55); margin-top: 2px; }
.lt-logos { display: flex; align-items: center; gap: 12px; }
.lt-logos img { height: 20px; filter: brightness(0) invert(1); opacity: 0.7; }
.lt-sep { width: 1px; height: 20px; background: rgba(255,255,255,0.25); }
```
**Customise:** `float-video-wrap` size (860×540 is 16:9 at ~70% viewport), speaker name/title.  
**Optional:** add timed bullet cards that appear after N seconds using `setTimeout(() => el.classList.add('show'), 55000)`.

---

### 3 · "Meet [Persona]"
*One scene. Persona intro with a central video and 3 signal cards on the right, staggered in.*

```html
<div class="meet-layout">
  <div class="meet-copy">
    <span class="eyebrow">Meet</span>
    <h1 class="headline">Sarah</h1>
  </div>
  <div class="meet-video-wrap">
    <video autoplay muted loop><source src="persona.mp4" type="video/mp4"></video>
  </div>
  <div class="signal-stack">
    <div class="signal-card glass-card" style="animation-delay:0.3s">
      <span class="sig-icon">💇</span>
      <span class="sig-label">Signal 1</span>
      <span class="sig-value">Values expert styling advice</span>
    </div>
    <div class="signal-card glass-card" style="animation-delay:0.55s">…</div>
    <div class="signal-card glass-card" style="animation-delay:0.8s">…</div>
  </div>
</div>
```
```css
.meet-layout {
  position: fixed; inset: 0; z-index: 20;
  display: grid; grid-template-columns: 260px 1fr 320px;
  align-items: center; gap: 32px; padding: 80px 60px 70px;
}
.eyebrow {
  display: block; font-size: 0.65rem; font-weight: 700;
  letter-spacing: 0.18em; text-transform: uppercase; color: var(--sf-blue); margin-bottom: 8px;
}
.headline { font-size: clamp(3.5rem, 7vw, 6rem); font-weight: 900; letter-spacing: -0.04em; color: #fff; }
.meet-video-wrap { border-radius: 16px; overflow: hidden; box-shadow: 0 24px 60px rgba(0,0,0,0.6); height: 70vh; }
.meet-video-wrap video { width: 100%; height: 100%; object-fit: cover; }
.signal-stack { display: flex; flex-direction: column; gap: 16px; }
.signal-card {
  padding: 18px 20px; display: flex; flex-direction: column; gap: 4px;
  animation: slideIn 0.6s ease both;
}
.sig-icon  { font-size: 1.4rem; }
.sig-label { font-size: 0.6rem; font-weight: 700; letter-spacing: 0.14em; text-transform: uppercase; color: var(--sf-blue); }
.sig-value { font-size: 0.85rem; font-weight: 500; color: rgba(255,255,255,0.85); }
```

---

### 4 · Customer Profile Card
*Multi-state. 3-column glass card revealing avatar, stats, and timeline.*

**Column layout:**
```
220px (avatar + pills) | 1fr (stats + product tiles) | 180px (activity timeline)
```

```html
<div class="profile-card glass-card">
  <!-- LEFT: avatar -->
  <div class="col-avatar">
    <div class="avatar-wrap state-1">
      <img src="persona.jpg" class="avatar-img" id="avImg">
    </div>
    <div class="persona-name state-1">Sarah Malik</div>
    <div class="segment-pills state-2">
      <span class="pill known">Known</span>
      <span class="pill loyal">Loyal</span>
    </div>
  </div>

  <!-- MIDDLE: stats + products -->
  <div class="col-mid">
    <div class="stat-grid state-2">
      <div class="stat-block"><span class="stat-val">£2,840</span><span class="stat-lbl">LTV</span></div>
      <div class="stat-block"><span class="stat-val">3</span><span class="stat-lbl">Products</span></div>
      <div class="stat-block"><span class="stat-val">94</span><span class="stat-lbl">NPS</span></div>
    </div>
    <div class="product-rail state-3">
      <div class="product-tile"><img src="product1.png"><span>Product A</span></div>
      <div class="product-tile"><img src="product2.png"><span>Product B</span></div>
    </div>
  </div>

  <!-- RIGHT: timeline -->
  <div class="col-timeline">
    <div class="timeline-item state-3" style="animation-delay:0.1s">
      <span class="t-dot known"></span>
      <div><span class="t-label">Purchased</span><span class="t-val">V10 Absolute</span></div>
    </div>
    <!-- more items -->
  </div>
</div>
```

**State machine:**
- State 0 → nothing shown
- State 1 → avatar fades in, name appears
- State 2 → segment pills + stat grid appear; avatar un-blurs if you started blurred
- State 3 → product rail + timeline animate in

**Avatar reveal (optional blur-to-reveal):**
```css
.avatar-img { filter: blur(12px) grayscale(1) brightness(0.55); transition: filter 1.2s ease; }
.revealed .avatar-img { filter: none; }
```
Add `.revealed` to `.avatar-wrap` in `goState(2)`.

**Pill styles:**
```css
.pill { padding: 4px 10px; border-radius: 100px; font-size: 0.6rem; font-weight: 700; letter-spacing: 0.1em; text-transform: uppercase; }
.pill.known  { background: rgba(16,185,129,0.15); color: var(--known); border: 1px solid rgba(16,185,129,0.3); }
.pill.loyal  { background: rgba(0,161,224,0.12);  color: var(--sf-blue); border: 1px solid rgba(0,161,224,0.25); }
.pill.anon   { background: rgba(255,255,255,0.06); color: rgba(255,255,255,0.5); border: 1px solid rgba(255,255,255,0.12); }
```

---

### 5 · Multi-state Profile (with overlays)
*Extends type 4 with a secondary panel that slides in over the profile card.*

**Pattern:** profile card shifts left using CSS transition, overlay panel slides in from right.

```css
.profile-card { transition: transform 0.6s cubic-bezier(0.16,1,0.3,1); }
.profile-card.split { transform: translateX(-240px); }

.overlay-panel {
  position: absolute; right: 0; top: 0; width: 440px; height: 100%;
  opacity: 0; transform: translateX(60px);
  transition: opacity 0.5s ease, transform 0.6s cubic-bezier(0.16,1,0.3,1);
  pointer-events: none;
}
.overlay-panel.show { opacity: 1; transform: translateX(0); pointer-events: auto; }
```

**Overlay types available:**
- **Gmail window** — macOS chrome dots + sidebar + email thread as HTML
- **iPhone push notification** — `.phone-shell` with `iphone-frame.png` background + notification card
- **Product page iframe** — `<iframe src="product.html" …>`
- **Data hub card** — list of signal/event rows with icons and timestamps

---

### 6 · Product Page
*Scrollable (or fixed) full product page for a single item with personalised pricing.*

**Layout:** two-column hero (`1fr 1fr`) — video/image left, content right.

```html
<div class="product-hero">
  <div class="hero-media">
    <video autoplay muted loop><source src="product.mp4" type="video/mp4"></video>
  </div>
  <div class="hero-content">
    <div class="personalisation-bar">
      <img src="persona.jpg" class="pers-avatar">
      <span>Your trade-in is already applied, Sarah</span>
    </div>
    <span class="eyebrow">Early Access · Loyalty Offer</span>
    <h1 class="product-title">Product Name</h1>
    <div class="pricing-card glass-card">
      <div class="price-row"><span>RRP</span><span class="strike">£599.99</span></div>
      <div class="price-row"><span>Trade-in credit</span><span class="deduct">−£120.00</span></div>
      <div class="price-row"><span>Loyalty discount</span><span class="deduct">−£40.00</span></div>
      <div class="price-row total"><span>Your price</span><span class="final">£439.99</span></div>
    </div>
    <button class="cta-btn" onclick="openCheckout()">Add to bag</button>
  </div>
</div>
```

**Checkout modal** (shown on CTA click):
```html
<div class="checkout-modal glass-card" id="checkoutModal">
  <div class="checkout-header">Secure Checkout</div>
  <div class="checkout-form">
    <!-- Pre-filled name, address, payment last 4 -->
    <div class="field-row"><label>Name</label><input value="Sarah Malik" readonly></div>
    …
  </div>
  <button class="confirm-btn" onclick="showConfirmation()">Confirm Order</button>
</div>
```

---

### 7 · Text Beat
*Multi-state. Full-screen staggered text reveals. Used for punchy statements.*

```html
<div class="beat-wrap">
  <div class="beat-eyebrow state-1">The challenge</div>
  <h1 class="beat-headline state-1">Every customer looks<br>the same until they don't.</h1>
  <div class="beat-body glass-card state-2">
    <p>When a customer lands without logging in, you're flying blind.
       Data Cloud changes that — signals surface the person behind the session.</p>
  </div>
  <div class="beat-kicker state-3">This is what identity resolution looks like.</div>
</div>
```
```css
.beat-wrap {
  position: fixed; inset: 0; z-index: 20;
  display: flex; flex-direction: column; justify-content: center;
  padding: 0 12vw; gap: 28px;
}
.beat-eyebrow {
  font-size: 0.65rem; font-weight: 700; letter-spacing: 0.18em;
  text-transform: uppercase; color: var(--sf-blue);
  opacity: 0; transition: opacity 0.4s ease;
}
.beat-headline {
  font-size: clamp(2.8rem, 5.5vw, 4.5rem); font-weight: 900;
  letter-spacing: -0.04em; line-height: 1.05; color: #fff;
  opacity: 0; transform: translateY(16px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.beat-body {
  max-width: 640px; padding: 24px 28px;
  font-size: 1.05rem; font-weight: 300; line-height: 1.6; color: rgba(255,255,255,0.75);
  opacity: 0; transform: translateY(12px);
  transition: opacity 0.5s ease 0.1s, transform 0.5s ease 0.1s;
}
.beat-kicker {
  font-size: 1.1rem; font-weight: 600; color: var(--sf-blue);
  opacity: 0; transition: opacity 0.4s ease;
}
/* Show states */
.state-1.show .beat-eyebrow,
.state-1.show .beat-headline { opacity: 1; transform: translateY(0); }
```
*Simpler pattern: add `.show` class to the correct elements in `goState()` rather than using visibility on wrapper.*

---

### 8 · Stats Display
*One scene (or state-revealed). Glass card with 2–4 large metric numbers.*

```html
<div class="stats-card glass-card">
  <div class="stats-eyebrow">Results · 90 days post-launch</div>
  <div class="stats-grid">
    <div class="stat-item">
      <span class="stat-number">+13%</span>
      <span class="stat-cloud">B2C Commerce</span>
      <span class="stat-desc">Conversion rate</span>
    </div>
    <div class="divider-v"></div>
    <div class="stat-item">…</div>
    <div class="divider-v"></div>
    <div class="stat-item">…</div>
  </div>
</div>
```
```css
.stats-card {
  position: fixed; top: 50%; left: 50%; transform: translate(-50%,-50%);
  padding: 52px 64px; z-index: 20; text-align: center;
}
.stats-eyebrow {
  font-size: 0.65rem; letter-spacing: 0.16em; text-transform: uppercase;
  color: rgba(255,255,255,0.45); margin-bottom: 36px;
}
.stats-grid { display: flex; align-items: center; gap: 48px; }
.stat-number {
  display: block; font-size: 4.5rem; font-weight: 900;
  letter-spacing: -0.04em; color: var(--sf-blue);
}
.stat-cloud { display: block; font-size: 0.6rem; font-weight: 700; letter-spacing: 0.14em; text-transform: uppercase; color: rgba(255,255,255,0.4); margin: 6px 0 4px; }
.stat-desc  { display: block; font-size: 0.9rem; font-weight: 400; color: rgba(255,255,255,0.7); }
.divider-v  { width: 1px; height: 80px; background: rgba(255,255,255,0.12); }
```

---

### 9 · Split Screen
*One scene. Two video panels (50/50) with a centre divider and optional content card.*

```html
<div class="split-layout">
  <div class="split-panel left">
    <video autoplay muted loop><source src="left.mp4" type="video/mp4"></video>
    <div class="panel-overlay"></div>
    <div class="panel-logo"><img src="brand-logo.png"></div>
    <div class="panel-label">Storefront Experience</div>
  </div>

  <div class="split-divider">
    <div class="split-badge">×</div>
  </div>

  <div class="split-panel right">
    <video autoplay muted loop><source src="right.mp4" type="video/mp4"></video>
    <div class="panel-overlay"></div>
    <div class="panel-logo"><img src="salesforce-cloud.png"></div>
    <div class="panel-label">Agentforce</div>
  </div>

  <!-- optional centre content card -->
  <div class="centre-card glass-card">
    <h2>Ship faster.</h2>
    <ul>
      <li>30-minute onboarding</li>
      <li>Live inventory sync</li>
      <li>Zero lock-in</li>
    </ul>
  </div>
</div>
```
```css
.split-layout { position: fixed; inset: 0; display: grid; grid-template-columns: 1fr 2px 1fr; z-index: 20; }
.split-panel { position: relative; overflow: hidden; }
.split-panel video { position: absolute; inset: 0; width: 100%; height: 100%; object-fit: cover; }
.panel-overlay { position: absolute; inset: 0; background: rgba(0,0,0,0.5); }
.panel-logo { position: absolute; top: 36px; left: 36px; }
.panel-logo img { height: 26px; filter: brightness(0) invert(1); }
.panel-label {
  position: absolute; bottom: 72px; left: 36px;
  font-size: 0.65rem; font-weight: 700; letter-spacing: 0.16em; text-transform: uppercase; color: rgba(255,255,255,0.5);
}
.split-divider { background: rgba(255,255,255,0.15); position: relative; }
.split-badge {
  position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%);
  width: 48px; height: 48px; border-radius: 50%;
  background: rgba(8,8,8,0.9); border: 1px solid rgba(255,255,255,0.2);
  display: flex; align-items: center; justify-content: center;
  font-size: 1.2rem; color: rgba(255,255,255,0.5);
}
.centre-card {
  position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%);
  padding: 36px 40px; width: 320px; z-index: 25;
}
```

---

### 10 · Journey / Pipeline
*Multi-state. Step-by-step flow visualisation — customer journey, pipeline, or timeline.*

**Core pattern:** a row of `step-node` elements, each with a connecting `step-line`, animated in sequence.

```html
<div class="journey-wrap">
  <div class="journey-steps">
    <div class="step-node state-1" id="s1">
      <div class="step-icon anon">?</div>
      <div class="step-label">Anonymous</div>
      <div class="step-sub">Session start</div>
    </div>
    <div class="step-line state-2"></div>
    <div class="step-node state-2" id="s2">
      <div class="step-icon signal">⚡</div>
      <div class="step-label">Signal</div>
      <div class="step-sub">3 page views</div>
    </div>
    <div class="step-line state-3"></div>
    <div class="step-node state-3" id="s3">
      <div class="step-icon known">✓</div>
      <div class="step-label">Known</div>
      <div class="step-sub">Identity resolved</div>
    </div>
  </div>
</div>
```
```css
.step-node { display: flex; flex-direction: column; align-items: center; gap: 8px; opacity: 0; transform: scale(0.85); transition: opacity 0.4s ease, transform 0.4s ease; }
.step-node.show { opacity: 1; transform: scale(1); }
.step-icon { width: 56px; height: 56px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1.2rem; font-weight: 700; }
.step-icon.anon  { background: rgba(255,255,255,0.06); color: rgba(255,255,255,0.5); border: 1px solid rgba(255,255,255,0.12); }
.step-icon.signal{ background: rgba(245,158,11,0.15); color: var(--signal); border: 1px solid rgba(245,158,11,0.3); }
.step-icon.known { background: rgba(16,185,129,0.15); color: var(--known); border: 1px solid rgba(16,185,129,0.3); }
.step-line { flex: 1; height: 2px; background: rgba(255,255,255,0.1); align-self: center; transform-origin: left; transform: scaleX(0); transition: transform 0.5s ease; }
.step-line.show { transform: scaleX(1); }
```

---

### 11 · Outro
*Multi-state. Closing slide — logo lockup → platform pillars light up → closer statement.*

```html
<div class="outro-wrap">
  <!-- State 1: logo lockup -->
  <div class="outro-lockup state-1">
    <img src="brand-logo.png" class="outro-logo-brand">
    <span class="outro-x">×</span>
    <img src="salesforce-cloud.png" class="outro-logo-sf">
    <div class="outro-glow"></div>
  </div>

  <!-- State 2: platform pillars -->
  <div class="pillars state-2">
    <div class="pillar" id="p1">B2C Commerce</div>
    <div class="pillar" id="p2">Storefront Next</div>
    <div class="pillar" id="p3">Agentforce</div>
    <div class="pillar" id="p4">Data Cloud</div>
  </div>

  <!-- State 3: closer -->
  <div class="closer state-3">
    <h1>Let's build this together.</h1>
    <p>salesforce.com/brand</p>
  </div>
</div>
```
```css
.pillar {
  padding: 20px 32px; border-radius: 12px; font-size: 0.8rem; font-weight: 700;
  letter-spacing: 0.1em; text-transform: uppercase; color: rgba(255,255,255,0.4);
  border: 1px solid rgba(255,255,255,0.08); transition: all 0.5s ease;
}
.pillar.lit {
  background: rgba(0,161,224,0.14); color: var(--sf-blue);
  border-color: rgba(0,161,224,0.3); box-shadow: 0 0 24px rgba(0,161,224,0.15);
}
.outro-glow {
  position: absolute; width: 400px; height: 400px; border-radius: 50%;
  background: radial-gradient(circle, rgba(0,161,224,0.18) 0%, transparent 65%);
  pointer-events: none;
}
```
**Light pillars up in sequence:**
```js
function goState(n) {
  if (n === 2) {
    ['p1','p2','p3','p4'].forEach((id, i) => {
      setTimeout(() => document.getElementById(id).classList.add('lit'), i * 400);
    });
  }
}
```

---

## Asset substitution guide

| Original (Dyson) | Replace with |
|---|---|
| `dyson-logo.png` | `[brand]-logo.png` |
| `salesforce-cloud.png` | keep as-is |
| `iphone-frame.png` | keep as-is (generic) |
| `[persona].jpg` headshots | client or stock persona photos |
| background `.mp4` | lifestyle/product footage for the brand |
| `V10 Absolute`, `Airwrap` | brand's product names |
| `£` prices | local currency |
| `Dyson.co.uk` in emails | brand domain |
| Salesforce cloud names in strip | adjust per demo focus |

---

## Recommended deck order

A standard 8–12 screen deck flows as:

1. **Intro** (splash — brand × Salesforce)
2. **Talking head** (client speaker intro)
3. **Text beat** (problem/opportunity statement)
4. **"Meet [Persona A]"** (introduce existing customer)
5. **Profile card** for Persona A
6. **Multi-state profile** — email/notification overlay
7. **Product page** (personalised offer)
8. **"Meet [Persona B]"** (new or anonymous customer)
9. **Profile card** for Persona B with journey
10. **Stats display** (results / outcomes)
11. **Split screen** (technology pairing)
12. **Outro** (pillars + closer)

Add journey, pipeline, or social screens to support the narrative between steps.

---

## Generation workflow (for Claude)

When asked to build a demo deck for a new brand, follow these steps:

1. **Gather brand brief** — ask for: brand name, primary/accent colors, persona names and segments, product names, SF cloud focus, any asset file names already available.
2. **Generate shared assets file** — create `_shared.css` with all tokens, or embed inline since each file is standalone.
3. **Build screens in order** — Intro first (simplest), then profile cards, then complex overlays. Confirm video/image filenames before referencing them.
4. **Test state machines** — every multi-state screen must have an initial `goState(0)` call at the bottom of the script to set correct starting visibility.
5. **Verify z-index stack** — background (0) → overlay (1) → logos (10) → content (20) → strip (30) → modals (40+).
6. **Name files consistently** — `Intro.html`, `MeetSophie.html`, `SophieProfile.html`, `ProductPage.html`, `Stats.html`, `Outro.html`.
