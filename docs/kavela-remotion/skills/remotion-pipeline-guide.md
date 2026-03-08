---
title: "Remotion Pipeline Guide"
description: "How to use the autonomous Remotion pipeline — scheduling, rendering, and creating new compositions"
domain: "engineering"
keywords: ["remotion", "pipeline", "autonomous", "cron", "launchd", "render", "video", "automation", "claude code", "agent"]
author: "Yong"
workspace: "Kavela"
version: "1.0.0"
listing_type: "skill"
listing_url: "https://kavela.ai/marketplace/kavela-remotion"
category: "docs"
---
# Remotion Pipeline Guide

# Remotion Pipeline Guide

## Project Location
`kavela/remotion/` (flat — package.json is at this level)

## Quick Start
```bash
cd kavela/remotion
npm install
npm start          # Open Remotion Studio (preview)
npm run render:demo # Render product demo to out/
```

## Autonomous Pipeline
- `pipeline/orchestrator.mjs` — detect frontend changes → invoke Claude Code CLI → render
- `pipeline/schedule.mjs install` — macOS launchd nightly job at 23:00
- Uses `claude -p` headless with Claude Code Max subscription (local)
- State tracking in `pipeline/.last-run.json`

## Creating New Compositions
1. Create in `src/compositions/YourComp.tsx`
2. Use templates from `src/templates/` and tokens from `src/design/tokens`
3. Register in `src/Root.tsx`
