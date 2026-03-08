---
title: "Trailer Production Workflow"
description: "Step-by-step operational playbook for producing Remotion trailers — intake questions, scene-by-scene build order, timing calibration checklist, and hard-won production rules from real iterations"
domain: "engineering"
keywords: ["remotion", "trailer", "workflow", "production", "animation", "timing", "intake", "checklist", "video", "iteration", "storyboard", "scenes"]
author: "Yong"
workspace: "Kavela"
version: "1.0.0"
listing_type: "skill"
listing_url: "https://kavela.ai/marketplace/kavela-remotion"
category: "docs"
---
# Trailer Production Workflow

# Trailer Production Workflow

## Phase 0: Intake Questions

Ask these BEFORE writing any code. Keep it short — 6 questions max.
Each question has a **default** based on the Kavela Product Demo trailer (our first production).

### Required
1. **What's the core message?** One sentence. What should viewers feel/know after 30s?
   > Default: "Your AI tools are only as good as the context you give them — Kavela gives every tool the same brain."

2. **Who's the audience?** (developers, founders, general public, investors)
   > Default: Developers and engineering teams who use AI coding tools (Claude Code, Cursor, Windsurf, VS Code + Copilot).

3. **What's the hook stat or pain point?** The scroll-stopping opener. Must be specific.
   > Default: "Your team's knowledge lives in 6 different places. Your AI can access zero of them." → "5 devs. 5 context files. 5 different AI outputs."

4. **What are the 2-3 key outcomes to show?** Not features — outcomes.
   > Default:
   > - **Define once** — team writes context together via create_skill (collaborative dashboard)
   > - **Apply everywhere** — developer asks AI a question, check_context fires automatically, standards applied without thinking
   > - **Works in every AI tool** — same context across Claude, Cursor, VS Code, Windsurf, ChatGPT

5. **What platform icons/logos are needed?** Provide as SVG in `public/images/`.
   > Default: `claude.svg`, `cursor.svg`, `vscode.svg`, `windsurf.svg`, `openai.svg` (for ChatGPT). Plus `icon.png` (Kavela logo — already exists).

6. **CTA details:** URL, tagline, incentive line, any social proof stats.
   > Default:
   > - URL: **kavela.ai**
   > - Tagline: **"One brain. Every AI tool."**
   > - Incentive: **"Free early access signups starts now"**
   > - No social proof stats yet (pre-launch)

### Optional (have defaults)
- **Duration:** 43s (~1286 frames at 30fps). Twitter optimal: 30-45s.
- **Aspect ratio:** 1280×720 (Twitter primary). Also render 1920×1080 and 1080×1080 (Instagram).
- **Music BPM:** 128. Affects all beat-sync timing.
- **Design language:** Lemon.ai-style — white background (#ffffff), near-black text (#0f172a), Georgia serif for emotional text, Inter sans for UI, JetBrains Mono for CLI. Single blue accent (#3b82f6). Word-by-word reveals, spring physics, outcome pills. See "Motion Graphics Playbook" skill.

### Empathy slam (the emotional pivot)
> Default: "Your AI isn't broken." → "It's blind."
> Pattern: Validate the viewer ("not broken") then reframe the real problem ("blind") with italic + underline accent.

### Rapid outcome statements (one-idea-per-screen, 2s each)
> Default:
> 1. "New Hire, Full Context"
> 2. "Same standards. Every tool."
> 3. "Stop re-entering context."
> 4. "Start compounding it."

## Phase 1: Storyboard & Beat Map

### Build Order
1. Define the AIDA sections in `beat.ts` with beat markers
2. Scaffold all scene components as empty functions returning null
3. Wire them into the main composition in render order
4. Add `EXTENDED TIMING` constants for scenes that need to decouple from strict beat grid

### AIDA Time Budget (40s @ 128 BPM)
| Section | Beats | Time | Purpose |
|---------|-------|------|---------|
| A — Attention | 1-20 | 0-10s | Problem + chaos + empathy slam |
| I — Interest | 21-32 | 10-15s | Logo reveal + tagline |
| D — Desire | 33-64 | 15-30s | Use cases as outcomes |
| A — Action | 65-86 | 30-40s | Platform proof + CTA |

### Default Scene Lineup (9 scenes)
1. **LogoStamp** — icon.png snap in, hold briefly (beat 1-3)
2. **StatHook** — "Your team's knowledge lives in 6 different places..." (beats 4-14)
3. **FileChaos** — .cursorrules, CLAUDE.md etc float in + "5 devs. 5 context files. 5 different AI outputs." (beats 10-14)
4. **EmpathySlam** — "Your AI isn't broken." → "It's blind." with zoom out (beats 15-20)
5. **LogoReveal** — white flash → icon.png spring → "Kavela" mask wipe → "One brain. Every AI tool." (beats 21-32)
6. **ContextFlow** — Phase 1: "Define once." + terminal card. Phase 2: "Apply everywhere." + check_context demo with outcome pills (beats 33 → CONTEXT_END)
7. **PlatformOrbit** — "Works in every AI tool." + orbiting platform icons (ORBIT_IN → ORBIT_OUT)
8. **RapidOutcomes** — 4 statements, 2s each, uniform timing (RAPID_IN → CTA_IN)
9. **CTA** — logo + kavela.ai + tagline + incentive, hold 3.5s+ (CTA_IN → end)

### Extended Timing Pattern
The latter half of the video often needs exact breathing room that doesn't align to beats. Use computed frame constants:
```tsx
const CONTEXT_END = S.dashClick + 195;        // check_context scene + 1.1s dead end
const ORBIT_IN = CONTEXT_END - 4;             // 4-frame crossfade overlap
const ORBIT_OUT = ORBIT_IN + 120;             // ~4s visible
const RAPID_IN = ORBIT_OUT - 4;               // crossfade into rapid outcomes
const RAPID_SLOT = 60;                        // 2s uniform per statement
const CTA_IN = RAPID_IN + 4 * RAPID_SLOT - 5; // crossfade into CTA
```
Chain all late scenes this way. The 4-frame crossfade overlap prevents white gaps between scenes.

## Phase 2: Scene-by-Scene Build

Build scenes in AIDA order. For each scene:

1. **Entry condition** — `if (frame < start || frame >= exit) return null;`
2. **Fade envelope** — `fadeIn * fadeOut` as overall opacity
3. **Content animation** — WordReveal, springs, interpolations
4. **Exit timing** — Must end BEFORE next scene starts (or use crossfade overlap)

### Scene Architecture Rules
- Every scene is a function component: `function SceneName({ frame }: { frame: number })`
- Use `AbsoluteFill` for full-screen layout
- Keep scene lifespan tight — return null outside active range
- Never have two scenes at full opacity simultaneously (causes text overlap)

## Phase 3: Timing Calibration

This is where 80% of iteration happens. These rules prevent the most common issues:

### Dead End Rules
**Dead end** = time text stays fully visible and static before fading out.

| Content Type | Minimum Dead End |
|-------------|-----------------|
| Short phrase (3-4 words) | 0.5s (15 frames) |
| Medium sentence (5-7 words) | 0.75s (22 frames) |
| Long sentence (8+ words) | 1.0s (30 frames) |
| Stat with numbers | 1.0s (30 frames) |
| CTA / final screen | 3.5-4s (105-120 frames) |

### Word Reveal Timing
For `WordReveal` with stagger:
- **Reading completion frame** = `start + (wordCount - 1) * stagger + 5` (5 frames for last word's opacity ramp)
- **Fade out must not start before** reading completion + dead end
- Common mistake: fading out while last word is still animating in

### Stagger Values
| Content | Stagger (frames) |
|---------|-----------------|
| Punchy headline (1-4 words) | 4-5 |
| Standard sentence | 3-4 |
| Dense stat with numbers | 2 |

### Scene Transition Checklist
For every scene boundary, verify:
- [ ] Current scene's fadeOut completes BEFORE next scene's fadeIn starts (or intentional 4-frame crossfade)
- [ ] No two text elements at full opacity simultaneously
- [ ] Gap between scenes is < 6 frames (no visible white flash unless intentional)

### Shifting Scenes (the EXT pattern)
When you need to extend a scene's dead end and everything after must shift:
```ts
const EXT = 18; // 0.6s at 30fps
// Apply to ALL beat markers from the shifted point onward
slam1: beatToFrame(15) + EXT,
```
This is cleaner than adding offsets in the component file — all timing stays in beat.ts.

## Phase 4: Visual Polish

### Typography
- **Serif (Georgia)** for emotional/slam text
- **Sans (Inter)** for UI labels, captions
- **Mono (JetBrains Mono)** for CLI/code/terminal content
- Accent word: use `accentWord` prop with blue color for ONE key word per sentence

### Spring Configs
```tsx
bouncy: { damping: 10, mass: 0.4, stiffness: 260 }  // UI elements, logos
snappy: { damping: 12, mass: 0.5, stiffness: 200 }  // cards, sidebars
soft:   { damping: 15, mass: 0.8, stiffness: 120 }   // gentle fades
```

### Orbit Animation
Spring-based rotation (fast burst, decelerate):
```tsx
spr(frame, start, { damping: 20, mass: 1.5, stiffness: 60 })
```
NOT constant-speed rotation. The spring feels more dynamic.

### Looping Dot Animation (for loading states)
Ping-pong pattern using array index:
```tsx
const dots = [1, 2, 3, 2]; // ping-pong
const idx = Math.floor(elapsed / 8) % dots.length;
const dotStr = ".".repeat(dots[idx]);
```

### Icon Requirements
- Format: SVG preferred (scales cleanly), PNG acceptable at 256×256+
- Background: transparent
- Placement: `public/images/`
- Reference: `staticFile("images/name.svg")` with `<Img>` component

## Phase 5: Review Checklist

Before marking complete, verify each:

- [ ] **Mute test**: Watch on mute. Every message readable as text?
- [ ] **First-frame test**: Frame 0 is visually interesting (not blank white)
- [ ] **No overlaps**: Scrub through and check no two text blocks overlap
- [ ] **Dead ends**: Each text block holds long enough to read
- [ ] **Crossfades**: No jarring white gaps between scenes
- [ ] **CTA hold**: Final screen holds 3.5s+ minimum
- [ ] **Icons load**: All staticFile references resolve
- [ ] **3 aspect ratios**: 1280×720 (Twitter), 1920×1080, 1080×1080 (Instagram) all registered
- [ ] **TypeScript clean**: `npx tsc --noEmit` passes
- [ ] **Duration matches**: Root.tsx durationInFrames matches actual content end frame

## Hard-Won Rules (from real iterations)

1. **Text always needs more reading time than you think.** If in doubt, add 0.5s.
2. **FileChaos/overlay scenes MUST exit before slam/empathy text enters.** Text overlap is the #1 visual bug.
3. **"Define once → Apply everywhere" two-phase scenes need a 0.5s pause between phases.** Don't rush the transition.
4. **Looping dots must ping-pong** (. .. ... .. .), not just increment (. .. ...). The ping-pong feels alive.
5. **Platform orbit spin must be spring-based**, not constant rotation. Fast burst → decelerate.
6. **User prompts need visual identity** — avatar circle + chat bubble, not just quoted text.
7. **Terminal/CLI cards need generous padding** (44px+ top, 52px+ sides) and min-width 700px. Cramped terminals look amateur.
8. **Outcome pills need 20+ frame stagger between each.** Too fast = unreadable.
9. **CTA must show: logo + URL + tagline + incentive.** Missing any one feels incomplete.
10. **Always use kavela.ai** (not .dev, not .com). Verify copy in CTA.
