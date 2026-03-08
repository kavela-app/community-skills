# Motion Graphics Playbook

> Reverse-engineered rules for creating high-converting product demo videos — animation types, beat-sync patterns, storyboard structure, and what makes motion graphics feel premium

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Kavela)

**Keywords:** `motion graphics` `remotion` `animation` `video` `trailer` `product demo` `beat sync` `storyboard` `twitter` `marketing` `conversion`

[View on Kavela Marketplace](https://kavela.ai/marketplace/motion-graphics-playbook)

---
# Motion Graphics Playbook

## Core Principles

### What Makes It Feel Premium (Not Amateur)
1. **Variable timing** — NEVER use fixed-duration blocks. Fast things FAST (3-5 frames), breathing moments LONG (30-45 frames). The contrast is what creates energy.
2. **Ease with overshoot** — bezier(0.34, 1.56, 0.64, 1) for elements snapping into place. Creates "pop" without being cartoony.
3. **Camera drift** — Subtle continuous sinusoidal movement (2-3px amplitude) on the main container. Prevents static feeling.
4. **Anticipation + follow-through** — Slight pullback before big moves. Slight overshoot after landing. Disney's 12 principles apply.
5. **Beat sync** — Major transitions and entrances snap to musical beats. At 128 BPM = ~14 frames per beat at 30fps.
6. **Breathing room** — Moments of stillness between action. Let the viewer process. Don't fill every frame with movement.
7. **Depth** — 3D perspective tilts on entrance (5° → 0°), parallax on layers, scale differences.

### What NOT To Do
- Fixed 2.5s blocks for every animation
- Everything as a gradient
- Pure black backgrounds (kills detail on Twitter compression)
- Same easing on everything
- Too many effects simultaneously
- Copying UI pixel-for-pixel instead of being inspired by it

## Logo Rules
- **ALWAYS use icon.png** (colorful K — green, blue, yellow, orange)
- NEVER use Kavela_gradient.png as main logo
- Logo entrance: elastic spring (damping 8-10, stiffness 180-200)

## High Contrast Dark Mode
- Background: #111827 (slate-900, NOT pure black)
- Cards: #1e293b with borders #334155
- Headlines: #ffffff (PURE white)
- Body: #f1f5f9 (slate-100)
- Muted: #94a3b8 (slate-400)
- Keep brand colors vivid: blue #3b82f6, violet #8b5cf6, cyan #22d3ee

## 30 Animation Types

### Text Entrances
1. **Slam Down** — Drops from top with spring bounce + camera shake (3-frame settle)
2. **Word-by-Word** — Each word fades up, 3-frame stagger, varying speeds
3. **Character Scatter** — Letters start random positions, snap into place
4. **Mask Wipe** — clipPath reveal left-to-right
5. **Blur-to-Sharp** — filter: blur(20px) → blur(0) with scale 1.05→1
6. **Kinetic Typing** — Monospace typewriter, variable speed, backspace corrections
7. **Split Screen** — Two halves slide apart revealing text
8. **Counter Roll** — Digits roll like slot machine (for stats)
9. **Highlight Marker** — Text appears, then colored underline draws behind key words
10. **Glitch Flash** — 2-3 frame RGB split then settle

### UI/Object
11. **Parallax Float** — Layers at different speeds
12. **Zoom-Through** — Camera pushes through element into next scene
13. **Tilt Shift** — Selective blur on edges, miniature effect
14. **Orbit** — Elements rotate around center in 3D
15. **Elastic Snap** — Overshoot + spring back (rubber band)
16. **Morph Transform** — Shape morphs into another
17. **Particle Burst** — Dots explode from click point
18. **Perspective Push** — 3D tilt as if pushed, settles flat
19. **Screen Record Feel** — Cursor with natural wobble
20. **Line Draw** — SVG paths drawing themselves

### Transitions
21. **Iris Wipe** — Circular reveal from center
22. **Shutter Cut** — Horizontal slats
23. **Momentum Swipe** — Flies off with velocity blur
24. **Cross-Dissolve + Scale** — Crossfade with opposing scales
25. **Flash Cut** — Single white frame (opacity 0.06-0.9)
26. **Gravity Drop** — Scene falls away

### Rhythm/Impact
27. **Beat Pulse** — Scale 1.02 on beat, 1.0 off beat
28. **Fast Blink Text** — Flash on 4-5 frames, off 2 frames, repeat
29. **Freeze Frame + Label** — Action freezes, annotation slides in
30. **Speed Ramp** — Slow for hero moments, 2x for transitions

## Beat-Sync System

```
BPM = 128 (adjustable)
FRAMES_PER_BEAT = (60 / BPM) * FPS  // ~14 at 128bpm/30fps
beatToFrame(beat) = (beat - 1) * FRAMES_PER_BEAT
```

Align all major events to beat positions. Sub-beat events (stagger, cascades) use fractional beats.

## Storyboard Template (30s Twitter Cut)

| Bars | Beats | Frames | Scene | Energy |
|------|-------|--------|-------|--------|
| 1-2 | 1-8 | 0-112 | COLD OPEN — punch text | HIGH |
| 3-4 | 9-16 | 112-224 | LOGO + HOOK | MEDIUM |
| 5-8 | 17-32 | 224-448 | PRODUCT SHOWCASE | BUILDING |
| 9-12 | 33-48 | 448-672 | KEY FEATURE DEMO | PEAK |
| 13-16 | 49-64 | 672-900 | RAPID FIRE + CTA | CLIMAX → HOLD |

Energy curve: grab → breathe → build → peak → resolve.

## File Structure
```
src/lib/beat.ts           — BPM, beat-to-frame conversion, scene map
src/compositions/         — One file per video
src/templates/            — Reusable animation components
src/design/tokens.ts      — Colors, fonts, spacing
public/images/icon.png    — THE logo (colorful K)
```
