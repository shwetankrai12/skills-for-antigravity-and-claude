---
name: antigravity
description: >
  Full-stack micro-agency execution skill for building client-facing digital assets targeting hyper-local Indian markets (real estate brokers, gyms, local businesses). Use this skill whenever the user asks to build landing pages, service pages, WhatsApp bot scripts, client proposals, pitch decks, pricing bundles, outreach messages, onboarding SOPs, or agency-related automation for Nevelto or similar micro-agency work. Trigger this skill aggressively — if the user mentions a client, a niche (real estate, gym, restaurant), a deliverable (page, proposal, bot script), or says anything about agency work, outreach, or closing a deal, use this skill immediately. This skill encodes the full Nevelto agency workflow: prospect → pitch → deliver → retain.
---

# Antigravity — Nevelto Coding Execution Skill

You are building production assets for **Nevelto**, a hyper-local digital agency targeting the **Hooghly, West Bengal** market. Solo founder, 22 years old, financial constraints. Move fast. No unnecessary rebuilds. Ship clean code.

---

## What's Already Built — DO NOT Rebuild

### Landing Page (DigiLocal v1 — reference implementation)
A complete, production-ready agency landing page already exists. Key patterns to reuse:

**Design System (already established — use these vars)**
```css
:root {
  --ink: #0e0e0e;
  --paper: #f5f0e8;
  --lime: #c8f135;
  --orange: #ff5c2b;
  --blue: #1a1aff;
  --pink: #ff2d78;
  --white: #ffffff;
  --display: 'Syne', sans-serif;   /* headings */
  --body: 'DM Sans', sans-serif;   /* body text */
}
```
Fonts: `https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=DM+Sans:wght@300;400;500`

**Page structure (proven, reuse for all client sites):**
1. Fixed nav (logo + links + CTA button)
2. Hero (eyebrow pill → H1 with outline text effect → subtext → 2 CTAs → floating shapes)
3. Stats strip (dark bg, lime numbers)
4. Services grid (cards with top color border per card)
5. Process section (dark bg, numbered steps)
6. Niche section (broker / gym targeting)
7. CTA banner
8. Footer
9. Chat widget (keyword-matching bot)

**Reuse patterns:**
- Hero eyebrow: blinking orange dot + lime pill background
- H1 outline text: `-webkit-text-stroke: 2.5px var(--ink); color: transparent`
- Card hover: `transform: translateY(-6px); box-shadow: 6px 6px 0 var(--ink)`
- All buttons: `border-radius: 100px` (pill shape)
- Floating shapes: 3 circles, `animation: float 6s ease-in-out infinite`

### Chat Widget (already built — copy and customize)
Keyword-matching bot embedded in HTML. Structure:

```javascript
const BOT_QA = [
  {
    keywords: ["hi","hello","namaste"],
    reply: "Message text\n\nWith line breaks",
    chips: ["Quick Reply 1", "Quick Reply 2"]
  }
];
const DEFAULT_REPLY = { reply: "...", chips: [...] };
```

**To customize for a new client:** only edit `BOT_NAME` and the `BOT_QA` array. Core logic (toggleChat, initChat, handleUserInput, getResponse) is stable — do not touch.

**Standard QA topics every client bot must have:**
- Greeting → chips to main topics
- Pricing → exact packages + payment terms (50/50)
- Services → what's included, delivery time ("2 ghante mein")
- How it works → numbered steps
- Niche-specific (broker / gym keywords)
- Contact → WhatsApp number + hours
- Results/guarantee → proof points

### Nevelto.bot (WhatsApp bot — v2.0.0, production stable)
Stack: Node.js + `whatsapp-web.js` + `groq-sdk` + `express` + `socket.io` + `pino` + `moment`

**Already done — never rebuild:**
- LocalAuth session persistence across restarts
- Slash commands: `/start`, `/help`, `/ping`, `/status`
- Groq AI (Llama 3.1 8B instant) integration
- Express server + Socket.IO real-time dashboard
- QR generation + ready/disconnect event handling
- JSON activity logger (last 100 interactions, persisted)
- Landing page + dashboard properly separated

**For each new client bot — only change:**
1. Groq system prompt (business context)
2. LocalAuth session folder: `sessions/[client-slug]`
3. Custom slash commands if needed
4. New Render service per client (free tier, one active bot per service)

**Multi-client scaling (when Render free tier maxes out):** PM2 on a single DigitalOcean droplet (₹500/month handles 5–10 concurrent bots).

---

## Agency Context

**Service Bundles**
| Bundle | Price | Deliverables |
|--------|-------|--------------|
| Starter | ₹5,000 | Google Maps setup + WhatsApp bot |
| Growth | ₹7,500 | Maps + 1-page website + bot |
| Full Bundle | ₹10,000 | Maps + website + AI bot (Groq) + logo |

Payment: 50% upfront UPI, 50% on delivery. No contracts.

**Primary niches:** Real estate brokers, gyms. Secondary: restaurants, coaching institutes.

**Hosting:** Netlify (frontend, free), Render (bot backend, free tier).

---

## Building Client Landing Pages

When asked to build a client site, follow this order:

**1. Ask only what's missing:**
- Business name + niche
- Phone number for WhatsApp CTA
- Key services to highlight (3–4)
- Any brand colors? (else use DigiLocal defaults)

**2. Adapt from DigiLocal base — not from scratch:**
- Keep CSS variable system + font imports unchanged
- Swap: brand name, hero headline, services, stats, chat QA, WhatsApp number
- Update `<title>` → `[Business Name] — [Area], Hooghly`
- Add `<meta name="description">` with local SEO keywords

**3. Niche-specific adjustments:**

*Real estate broker:*
- Hero: trust + local authority ("Hooghly ka sabse trusted broker")
- Stats: properties listed, years, happy clients
- Chat keywords: room, flat, rent, plot, pg, hostel, property, broker
- Color: keep orange or swap to deep blue

*Gym/fitness:*
- Hero: energy + transformation ("Apni best body banao")
- Stats: members, trainers, sq ft, years open
- Chat keywords: gym, fitness, membership, timing, fees, trainer, yoga
- Consider dark variant: swap `--paper: #111`, `--ink: #f5f0e8`

**4. WhatsApp float button (standard — paste before `</body>`):**
```html
<a href="https://wa.me/91XXXXXXXXXX?text=Hi%2C+main+aapke+baare+mein+jaanna+chahta+hun"
   class="wa-float" target="_blank" rel="noopener">💬</a>
<style>
.wa-float {
  position: fixed; bottom: 90px; right: 24px;
  background: #25d366; color: white;
  width: 56px; height: 56px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 26px; box-shadow: 0 4px 14px rgba(37,211,102,.5);
  text-decoration: none; z-index: 999;
  animation: wapulse 2s ease-in-out infinite;
}
@keyframes wapulse { 0%,100%{box-shadow:0 4px 14px rgba(37,211,102,.5)} 50%{box-shadow:0 4px 24px rgba(37,211,102,.8)} }
</style>
```

---

## Groq Bot System Prompt Template

For AI-powered WhatsApp bots (Full Bundle clients):

```
You are the WhatsApp assistant for [Business Name], a [niche] in [Area], Hooghly, West Bengal.
Your job: answer inquiries and get the customer to book/visit/call.

Rules:
- Reply in Hinglish (mix of Hindi + English), friendly, max 60 words per reply
- Pricing: give the range, say "exact quote aapko call pe denge"
- Complaints: "Iske liye directly humse baat karo: [phone]"
- Never make up facts. Never discuss competitors.
- End every reply with a soft CTA: "Visit karo?", "Call karein?", or "Aur kuch?"
- If confused: "Main samjha nahi — [phone] pe call karein!"

Business info:
Name: [Business Name]
Location: [Address], Hooghly
Timings: [Days, Hours]
Services: [3–4 key services]
Contact: [WhatsApp/Phone number]
```

---

## Code Rules (Non-Negotiable)

- **No frameworks.** Vanilla HTML/CSS/JS. Works without npm, without a build step.
- **Single `.html` file** per client site. One file = easy to hand off, deploy, edit.
- **No CDN dependencies** beyond Google Fonts.
- **Mobile-first.** Base styles for 375px, then scale up with `@media (min-width: 768px)`.
- **Sub-2s load.** No unoptimized images. No blocking scripts.
- **All `<script>` tags** at bottom of `<body>`.
- **CSS variables** for all brand colors — client color change = 1 minute.

---

## Pre-Delivery Checklist

Before handing off any client site:
- [ ] WhatsApp float links to correct number
- [ ] Chat widget responds to "hi", "price", "contact"
- [ ] Phone numbers in page are correct
- [ ] Google Maps embed present
- [ ] `<title>` is `[Business Name] — [Area], Hooghly`
- [ ] `<meta name="description">` present with local keywords
- [ ] No console errors
- [ ] Tested at 375px mobile width

---

## Monthly Retention Message (WhatsApp)

Send on 1st of each month:
```
📊 [Month] Report — [Business Name]

✅ Inquiries handled: [X]
🎯 Leads captured: [X]
⏱ Bot uptime: [X]%
❓ Top question: "[most asked]"

Kuch change karna ho toh reply karo 🙌
— Nevelto
```

Upsell trigger: leads > 10/month on Starter → pitch Growth Pack in the same message.

---

## The Goal

Every output moves toward one of:
1. New client paying ₹5k–₹10k
2. Delivered project → "yaar kya cheez bana diya"
3. Retained client who refers someone

Build fast. Ship clean. Collect payment. Repeat.
