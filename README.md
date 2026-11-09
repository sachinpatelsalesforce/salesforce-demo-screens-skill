# Salesforce B2C Demo Screens — Claude Skill

A Claude Code skill for generating self-contained HTML presentation screens for **Brand × Salesforce** demo videos. Each screen is a standalone full-viewport HTML file designed for screen capture into demo recordings.

---

## What this produces

A set of HTML screens that together form a complete demo deck — typically 8–12 screens covering a customer journey, with animated state reveals driven by keyboard/click navigation. Screens are built around real Salesforce cloud products (B2C Commerce, Storefront Next, Agentforce, Data Cloud) and customised per brand.

**Screen types included:**

| # | Type | Purpose |
|---|---|---|
| 1 | Splash / Intro | Brand × Salesforce logo lockup opener |
| 2 | Talking Head | Speaker intro with floating video + lower-third |
| 3 | Meet [Persona] | Persona intro with signal cards |
| 4 | Customer Profile Card | Multi-state card — avatar, stats, product tiles, timeline |
| 5 | Multi-state Profile + Overlay | Profile card + Gmail / iPhone notification / product page overlay |
| 6 | Product Page | Hero product page with personalised pricing + checkout modal |
| 7 | Text Beat | Full-screen staggered text for punchy statements |
| 8 | Stats Display | Large-number results card |
| 9 | Split Screen | Two-panel video split (e.g. brand vs. Salesforce) |
| 10 | Journey / Pipeline | Step-by-step customer journey flow |
| 11 | Outro | Logo lockup → platform pillars light up → closer |

---

## Setup

### 1. Add the skill to your Claude Code project

Copy `SKILL.md` into the `.claude/` folder at the root of your project:

```
your-project/
└── .claude/
    └── SKILL.md
```

Claude Code automatically loads skills from `.claude/SKILL.md` when you work in that directory.

### 2. Gather your brand assets

Before asking Claude to build screens, have these ready:

| Asset | Format | Notes |
|---|---|---|
| Brand logo | `brand-logo.png` | Will be white-inverted — SVG/PNG with transparency works best |
| Salesforce logo | `salesforce-cloud.png` | Included in standard Salesforce brand kit |
| Background video(s) | `.mp4` (looping, muted) | Lifestyle/product footage, 10–30s loops |
| Persona headshots | `persona-name.jpg` | 3–4 photos, portrait orientation |
| Product images | `product-name.png` | 2–3 hero shots |
| iPhone frame | `iphone-frame.png` | Optional — only needed for notification overlays |

### 3. Prepare your brand brief

Have these details ready to give Claude:

```
Brand name:         e.g. Nike
Primary colour:     e.g. #111111
Accent colour:      e.g. #E5161A
Personas:           e.g. Sophie (Loyal Owner), Tom (New Customer)
  - Per persona: 2–3 data signals, 2–3 product affinities
Products:           e.g. Air Max 90, Dri-FIT Running Jacket
Salesforce clouds:  e.g. B2C Commerce, Storefront Next, Agentforce, Data Cloud
Tagline / closer:   e.g. "Let's build this together."
```

---

## Usage

Once `SKILL.md` is in your `.claude/` folder, open Claude Code in your project and run:

```
Build a full demo deck using the SKILL.md.

Brand: Nike
Primary: #111111  Accent: #E5161A
Personas: Sophie (loyal owner, owns Air Max 90, Dri-FIT jacket), Tom (new customer, browsing running)
Products: Air Max 90, Dri-FIT Running Jacket, React Infinity Run
Salesforce clouds: B2C Commerce, Storefront Next, Agentforce, Data Cloud
Assets are in: ./assets/

Build in this order: Intro, MeetSophie, SophieProfile, MeetTom, TomJourney, ProductPage, Stats, Outro
```

Claude will generate one HTML file per screen, each self-contained and ready to open in a browser for recording.

---

## Recording tips

- Open each HTML file in a browser at **1920×1080** (use browser zoom or a screen resolution tool to set this exactly)
- Use **QuickTime** (Mac) or **OBS** to record the screen
- Navigate states with **Space / →** (next) and **←** (previous)
- Each `.mov` or `.mp4` clip per screen can be assembled in **Final Cut**, **Premiere**, or **ScreenFlow**

---

## Customising the skill

The `SKILL.md` file is plain text — you can edit it directly to:

- Add new screen types with your own HTML/CSS templates
- Change the recommended deck order
- Adjust default colours or the Salesforce cloud names in the bottom strip
- Add brand-specific instructions (e.g. "always use Helvetica Neue for this client")

---

## File naming convention

Claude will name files consistently:

```
Intro.html
MeetSophie.html
SophieProfile.html
MeetTom.html
TomJourney.html
ProductPage.html
Stats.html
SplitScreen.html
Outro.html
```

---

## Requirements

- [Claude Code](https://claude.ai/code) (CLI or desktop app)
- A modern browser (Chrome recommended for `backdrop-filter` support)
- Screen recording software
- Brand assets (logo, background video, persona photos)

---

## Skill file

See [`SKILL.md`](./SKILL.md) for the full skill — it contains all boilerplate, screen templates, and the generation workflow Claude follows.
