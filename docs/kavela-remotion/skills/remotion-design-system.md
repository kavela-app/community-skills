---
title: "Remotion Design System"
description: "Kavela's Remotion design tokens, animation patterns, and visual rules — synced from the frontend landing page"
domain: "engineering"
keywords: ["remotion", "design system", "colors", "fonts", "animation", "motion graphics", "video", "trailer", "brand", "tokens"]
author: "Yong"
workspace: "Kavela"
version: "1.0.0"
listing_type: "skill"
listing_url: "https://kavela.ai/marketplace/kavela-remotion"
category: "docs"
---
# Remotion Design System

# Remotion Design System

## Logo
- **ALWAYS use icon.png** (the colorful K — green, blue, yellow, orange)
- NEVER use Kavela_gradient.png for the main logo
- Kavela_gradient.png is only for subtle background accents if needed

## Color Palette (synced from frontend/landing)

**Brand Blue:** #3b82f6 (primary), #2563eb (buttons), #1d4ed8 (hover)
**Accent Violet:** #8b5cf6 (primary), #7c3aed (darker)

**Light Mode (Dashboard):** #f6f6f6 (bg), #fcfcfd (card), #e8e8eb (border), #0f0f12 (fg), #64646e (muted)
**Dark Mode (Terminal/CLI):** #030712 (bg), rgba(17,24,39,0.8) (chrome), #e5e7eb (text), #9ca3af (muted)

## Typography
- **Headings:** Inter, system-ui — bold, tight leading (1.15)
- **Code/Terminal:** JetBrains Mono, Fira Code — monospace
- **Hero text:** 72-128px, bold
- **Section titles:** 36-60px, semibold

## Animation Philosophy
- Variable timing — NOT fixed 2.5s blocks. Fast things fast (0.2s), breathing moments (1.5s)
- Easing: [0.22, 1, 0.36, 1] for standard, overshoot bezier(0.34, 1.56, 0.64, 1) for pops
- Not everything needs a gradient — let typography and timing tell the story
- Senior motion designer taste: ease in/out, anticipation, follow-through

## Video Formats
| Format | Width | Height | Use |
|--------|-------|--------|-----|
| landscape | 1920 | 1080 | YouTube, website |
| twitter | 1280 | 720 | Twitter/X |
| square | 1080 | 1080 | Instagram |
