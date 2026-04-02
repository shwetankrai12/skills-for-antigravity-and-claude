---
name: antigravity
description: >
  Full-stack micro-agency execution skill for building client-facing digital assets targeting hyper-local Indian markets (real estate brokers, gyms, local businesses). Use this skill whenever the user asks to build landing pages, service pages, WhatsApp bot scripts, client proposals, pitch decks, pricing bundles, outreach messages, onboarding SOPs, or agency-related automation for Nevelto or similar micro-agency work. Trigger this skill aggressively — if the user mentions a client, a niche (real estate, gym, restaurant), a deliverable (page, proposal, bot script), or says anything about agency work, outreach, or closing a deal, use this skill immediately. This skill encodes the full Nevelto agency workflow: prospect → pitch → deliver → retain.
---

# Antigravity — Nevelto Micro-Agency Execution Skill

You are executing work for **Nevelto**, a hyper-local digital agency targeting the **Hooghly, West Bengal** market. Clients are primarily real estate brokers and gym/fitness owners. Budget range: ₹5,000–₹10,000 per bundle. The builder is a 22-year-old solo founder operating under financial constraints with strong technical skills (Node.js, Python, HTML/CSS, WhatsApp automation via Baileys/whatsapp-web.js, Groq AI).

The goal of every deliverable is: **close a paying client or retain an existing one**.

---

## Agency Context

**Core Service Bundles**

| Bundle | Price | Deliverables |
|--------|-------|--------------|
| Starter Presence | ₹5,000 | 1-page landing site + WhatsApp CTA + Google Maps setup |
| Growth Pack | ₹7,500 | Starter + basic WhatsApp auto-reply bot + lead capture form |
| Power Pack | ₹10,000 | Growth + AI-powered WhatsApp bot (Groq) + monthly analytics report |

**Target Niches**
- Real estate brokers (property listings, lead gen, inquiry handling)
- Gym/fitness centers (membership promos, class schedules, trial bookings)
- Secondary: restaurants, coaching institutes, salons

**Tech Stack for Deliverables**
- Frontend: Vanilla HTML/CSS/JS (no frameworks — fast, hostable on Netlify/Vercel free tier)
- Bot: Node.js + whatsapp-web.js + Groq SDK (Llama 3.1 8B) — see Nevelto.bot v2.0.0
- Hosting: Netlify (frontend), Render free tier (bot backend)
- Analytics: Simple JSON logs + Socket.IO dashboard (already built)

---

## Workflow: Prospect → Pitch → Deliver → Retain

### 1. PROSPECT
When building outreach or identifying a lead:
- Always personalize for the specific niche (broker vs gym)
- Lead with a local credibility hook ("Hooghly mein businesses ke liye")
- Use WhatsApp-first outreach (not email) for local Indian SMBs
- Pain points to address:
  - Broker: "Clients call but don't remember you" → need a Google-indexed presence
  - Gym: "No-shows after inquiry" → need automated follow-up bot

### 2. PITCH
When writing a proposal or pitch message:
- Keep it under 150 words for WhatsApp
- Include: what problem you solve + one-line proof + clear price + one CTA
- Never list features — sell outcomes ("3x more callbacks", "zero manual replies")
- For formal proposals (PDF/doc): use a 3-section structure: Problem → Solution → Investment

### 3. DELIVER
When building a client deliverable:

**Landing Pages**
- Follow the frontend-design skill for visual quality
- Must include: Hero (headline + subtext + CTA) → Social Proof (3 lines) → Services → WhatsApp Float Button → Footer with Google Maps embed
- WhatsApp CTA link format: `https://wa.me/91XXXXXXXXXX?text=Hi%2C+I+found+you+online`
- Mobile-first. 90% of local Indian users are on mobile
- Aesthetic direction for real estate: clean, trust-conveying, warm tones (not dark)
- Aesthetic direction for gyms: high energy, bold typography, dark with accent colors

**WhatsApp Bot Scripts**
- Always define: greeting message, main menu (numbered options), FAQ auto-responses, lead capture flow (name + contact + intent), fallback to human handoff
- For AI-powered bots (Groq): system prompt must include business name, niche, tone (friendly but professional), and what NOT to answer (pricing negotiations, complaints — escalate to human)
- Bot persona should match the business (not generic "I am an AI")

**Onboarding SOPs**
- Write step-by-step in simple English (client may be non-technical)
- Include: how to scan QR for bot, how to edit their content, who to contact for issues

### 4. RETAIN
- Monthly: send a 5-line WhatsApp report (messages handled, leads captured, uptime)
- Upsell trigger: if client gets 10+ leads/month from Starter → pitch Growth Pack
- Referral ask: after 30 days of happy usage, ask for 1 broker/gym referral

---

## Output Standards

**Code Quality**
- All HTML must work without a build step (no React, no npm for client sites)
- CSS: use CSS variables for brand colors, mobile-first media queries
- JS: vanilla only, no jQuery, no CDN dependencies unless unavoidable
- Everything must load under 2 seconds on a 4G connection

**Copy Standards**
- Hinglish is acceptable for WhatsApp outreach but deliverables to clients should be in clean English or Hindi depending on client preference
- Never use corporate jargon in client-facing copy
- Headlines: benefit-first ("Get More Gym Members This Month")

**File Organization for Each Client**
```
clients/
└── [client-name]/
    ├── brief.md          (niche, contact, requirements, price agreed)
    ├── site/             (landing page HTML/CSS/JS)
    ├── bot/              (bot config, system prompt, menu flows)
    ├── proposal.md       (pitch sent)
    └── reports/          (monthly summaries)
```

---

## Nevelto.bot Integration Notes

The bot engine is already built (v2.0.0, production stable). For new clients:
1. Copy the base bot config
2. Update the Groq system prompt with client business context
3. Update slash commands if needed (`/status`, `/help` can be customized)
4. Set `LocalAuth` session folder to `sessions/[client-name]`
5. Deploy on Render — each client gets their own service (free tier = 1 active bot)

For multi-client scaling beyond Render free tier: use PM2 on a single VPS (₹500/month DigitalOcean droplet handles 5–10 concurrent bots easily).

---

## Quick Reference: Reusable Snippets

**WhatsApp Float Button (HTML)**
```html
<a href="https://wa.me/91XXXXXXXXXX?text=Hi%2C+I+want+to+know+more" 
   class="whatsapp-float" target="_blank" rel="noopener">
  <img src="https://cdn-icons-png.flaticon.com/512/124/124034.png" alt="WhatsApp" />
</a>
```
```css
.whatsapp-float {
  position: fixed; bottom: 24px; right: 24px;
  width: 56px; height: 56px;
  border-radius: 50%; overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.3);
  z-index: 9999;
}
```

**Groq Bot System Prompt Template**
```
You are [Business Name]'s WhatsApp assistant. 
Business type: [gym/real estate/other]. Location: [City, Area].
Be friendly, brief (under 60 words per reply), and helpful.
If asked about pricing, say: "Our team will share the best options for you directly."
If asked something you don't know, say: "Let me connect you with our team."
Never make up information. Never discuss competitors.
Always end with a CTA: "Want to book a visit?" or "Want more details?"
```

**5-Line Monthly Report Template (WhatsApp)**
```
📊 [Month] Report — [Business Name]

✅ Messages handled: [X]
🎯 Leads captured: [X]  
⏱ Bot uptime: [X]%
📈 Top query: "[most asked question]"

Need any changes? Just reply here.
```

---

## Reminder: The Goal

Every output from this skill should move toward one of:
1. A new client paying ₹5k–₹10k
2. A delivered project that makes the client say "yaar kya cheez bana diya"
3. A retained client who refers someone

Build fast. Ship clean. Collect payment. Repeat.
