# Skills for Antigravity and Claude

This repository contains specialized AI skills and prompts designed for **Claude** (Anthropic's AI assistant) and the **Antigravity** micro-agency workflow. These skills enhance productivity in debugging, automation design, and full-stack development for client-facing digital assets.

## Overview

The skills are organized into Markdown files, each defining a specific agent or workflow capability. They are tailored for:
- **Debugging**: Systematic error resolution in Node.js, Python, and full-stack projects.
- **Automation**: Workflow design for repetitive tasks, bots, and integrations.
- **Agency Execution**: Building digital assets for hyper-local Indian markets (real estate, gyms, etc.) via the Nevelto agency.

## Skills

### 1. Auto-Debugger (`auto-debugger.md`)
- **Purpose**: Systematic debugging agent for Node.js, Python, and full-stack projects.
- **Trigger**: Activate when encountering errors, bugs, or broken builds, especially in Nevelto/Antigravity projects (WhatsApp bots, landing pages, FastAPI backends, Render deployments).
- **Key Features**:
  - Classify errors (syntax, reference, runtime, dependency, config, API changes, logic bugs).
  - Hypothesis-driven debugging: minimal patches, validation, iteration.
  - Smart file scanning: only reads relevant files to avoid token waste.

### 2. Automation Architect (`automation_architect.md`)
- **Purpose**: Design automation workflows for lead capture, WhatsApp bots, form handling, reminders, data pipelines, etc.
- **Trigger**: Use for any "automate", "workflow", or repetitive task description.
- **Key Features**:
  - Maps problems to Trigger → Process → Output format.
  - Minimal stack preference (Node.js, Firebase, Baileys).
  - Concise output: GOAL → FLOW → COMPONENTS → EDGE CASES → OPTIMIZATION.

### 3. Antigravity Micro-Agency Execution (`microservice.md`)
- **Purpose**: Full-stack execution for Nevelto micro-agency, targeting hyper-local Indian markets (Hooghly, West Bengal).
- **Trigger**: For building landing pages, WhatsApp bots, proposals, pitch decks, outreach, or agency automation.
- **Key Features**:
  - Service bundles: Starter Presence (₹5,000), Growth Pack (₹7,500), Power Pack (₹10,000).
  - Target niches: Real estate brokers, gyms/fitness centers.
  - Workflow: Prospect → Pitch → Deliver → Retain.
  - Tech stack: Vanilla HTML/CSS/JS, Node.js + whatsapp-web.js + Groq AI, Netlify/Render hosting.

### 4. Antigravity Coding Execution (`microservice_coding.md`)
- **Purpose**: Coding implementation for Nevelto agency deliverables.
- **Trigger**: When building production assets (landing pages, bots, etc.).
- **Key Features**:
  - Reuses established design system and page structures from DigiLocal v1.
  - Focus on fast, clean code shipping.
  - Pre-built patterns: navigation, hero sections, service grids, chat widgets.

## How to Use

1. **For Claude Integration**: Copy the content of the relevant `.md` file into your Claude prompt or custom instruction set.
2. **Activation**: Each skill includes trigger conditions — use them to activate the appropriate agent based on the task.
3. **Customization**: Modify the skills as needed for your specific use cases, but maintain the core philosophy of efficiency and minimalism.

## Tech Stack & Dependencies

- **Frontend**: HTML, CSS, JavaScript (vanilla, no frameworks).
- **Backend/Bots**: Node.js, whatsapp-web.js, Groq AI (Llama 3.1 8B).
- **Hosting**: Netlify (frontend), Render (backend).
- **Analytics**: JSON logs, Socket.IO dashboards.

## Contributing

This repository is for personal use in the Antigravity/Nevelto workflow. If you'd like to contribute or suggest improvements, feel free to open an issue or pull request.

## License

All content is proprietary to the Nevelto agency workflow. Use at your own discretion.
