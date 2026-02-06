---
description: Create 3Blue1Brown-style mathematical animations with Manim - from concept to complete video
argument-hint: <topic or concept to visualize>
allowed-tools: Bash, Read, Write, Edit, Grep, Glob, WebSearch, WebFetch, Task
---

# Manim Video Composer

**Input:** $ARGUMENTS

Transform any concept into a polished, 3Blue1Brown-style animated explainer video. This skill handles the entire pipeline: research, planning, scripting, coding, and rendering.

---

## CRITICAL: Emotional Pacing (NOT Mechanical Timing)

### The #1 Rule: CONTRAST Creates Impact

Professional videos punch because they use **tension and release**:
- Fast cuts → SLOW for impact
- Buildup → PUNCH → HOLD → breathe
- Silence before reveals
- Variable pacing, NOT uniform timing

### Emotional Timing Vocabulary (Replaces Flat Fibonacci)

```python
# === EMOTIONAL TIMING (context-aware, not mechanical) ===

# Tension builders - accelerating sequence
TIMING_BUILDUP = [1.8, 1.2, 0.8, 0.5, 0.3, 0.15]  # Gets faster each step

# Impact moments
TIMING = {
    "punch": 0.12,           # SNAP into place (rush_into)
    "hold_impact": 2.5,      # Let it breathe after punch
    "dramatic_reveal": 1.8,  # Slow, deliberate appearance
    "settle": 0.6,           # Return to normal
    "breathe": 1.5,          # Pause for comprehension

    # Standard actions (use sparingly)
    "quick": 0.3,
    "normal": 0.6,
    "slow": 1.2,

    # Beat-synced (120 BPM default)
    "beat": 0.5,
    "half_beat": 0.25,
    "double_beat": 1.0,
}

# Easing functions for emotion
from manim import rate_functions

EASE = {
    "punch": rate_functions.rush_into,           # SNAP - fast start, hard stop
    "reveal": rate_functions.ease_out_expo,      # Dramatic slow reveal
    "bounce": rate_functions.ease_out_bounce,    # Playful landing
    "anticipate": rate_functions.ease_in_out_back,  # Pull back then go
    "smooth": rate_functions.smooth,             # Standard ease
    "linear": rate_functions.linear,             # Mechanical (use rarely)
}
```

### The Punch Pattern (USE THIS)

```python
def build_and_punch(self, buildup_items, climax_item):
    """
    Classic tension-release: accelerate through items,
    pause, PUNCH the climax, HOLD for impact.
    """
    # Accelerating buildup
    delays = [0.8, 0.5, 0.35, 0.25, 0.15]
    for i, item in enumerate(buildup_items):
        delay = delays[min(i, len(delays)-1)]
        self.play(FadeIn(item), run_time=delay)

    # Beat of silence before impact (CRITICAL)
    self.wait(0.4)

    # PUNCH - fast with hard stop
    self.play(
        FadeIn(climax_item, scale=1.15),
        run_time=0.12,
        rate_func=rate_functions.rush_into
    )

    # HOLD for impact (CRITICAL - this is where it lands)
    self.wait(2.0)
```

---

## CRITICAL: Style & Asset Rules

### USE ACTUAL LOGO FILES - NEVER APPROXIMATE

When using Means/JWS branding, ALWAYS download and use the actual PNG/image files:

```python
# CORRECT: Use ImageMobject with actual logo files
from manim import *

# Download logos first (run in bash):
# curl -o assets/MeansSig.png "https://c.jws.ai/MeansSigBlack.png"
# curl -o assets/JWSWhite.png "https://c.jws.ai/JWS%20Icon%20Only%20White.png"

# Then use ImageMobject:
means_logo = ImageMobject("assets/MeansSig.png").scale(0.5)
jws_logo = ImageMobject("assets/JWSWhite.png").scale(0.3)
self.play(FadeIn(means_logo))

# WRONG: Never try to recreate logos with SVG paths or Text approximations
# sig = Text("Means", font="Brush Script MT")  # NO! Use the actual image!
```

### Official Logo URLs (CDN)

| Logo | URL | Usage |
|------|-----|-------|
| Means Signature (Black) | `https://c.jws.ai/MeansSigBlack.png` | Dark backgrounds |
| Means Signature (White) | `https://c.jws.ai/MeansSigWhite.png` | Light backgrounds |
| JWS Icon (White) | `https://c.jws.ai/JWS%20Icon%20Only%20White.png` | Dark backgrounds |
| JWS Icon (Black) | `https://c.jws.ai/JWS%20Icon%20Only%20Black.png` | Light backgrounds |

### Fonts That Render Properly in Manim

```python
# SAFE FONTS - these render correctly:
FONT_BODY = "Helvetica Neue"      # Or "Arial" as fallback
FONT_CODE = "Menlo"               # Or "Courier New" as fallback
FONT_BOLD = "Helvetica Neue Bold"

# AVOID - may render incorrectly:
# "SF Pro Display" - system font, inconsistent
# "Brush Script MT" - cursive doesn't render well
# Custom/exotic fonts - use images instead
```

---

## Phase 0: Deep Research (REQUIRED for Means Projects)

**When visualizing Means/JWS products, ALWAYS spawn parallel Task agents to research:**

```
Launch these agents IN PARALLEL before writing any code:

1. Task(subagent_type="Explore"): Search codebase for [product] - architecture, features, code
2. Task(subagent_type="Explore"): Search JWS/JCS modules - technical capabilities
3. WebFetch: Scrape means.ai for latest project descriptions and visuals
4. WebFetch: Scrape product-specific sites (jws.ai, outtakes.com, voosey.com, etc.)
```

### Means Product Priority (when showcasing ecosystem)

**High Priority (feature prominently):**
1. **Mean-E** - Sovereign AI agent, LLM integration
2. **JWS** - Core platform, full-stack Swift
3. **Outtakes** - Creative AI, computer vision, generative
4. **Voosey** - Spatial computing, visionOS, architecture
5. **Neuraform** - Brainwave entrainment, audio processing

**Medium Priority:**
6. **Neurafund** - Finance AI, investment intelligence
7. **RevoluSun** - Solar CRM, renewable energy

**Lower Priority:**
8. **GHF** - Global Housing Foundation
9. **Untold Culture** - Digital wellness

### Supported Platforms (INCLUDE WEB!)

```
iOS • iPadOS • macOS • visionOS • Web • Linux • Embedded • tvOS
```

**Web is a first-class platform** - JWS powers means.ai, jws.ai, outtakes.com, voosey.com

---

## Phase 1: Research & Understand

Before asking questions, deeply research the topic:

1. **Spawn parallel agents** to search codebase and web
2. **Identify the "aha moment"** - what makes this click for learners
3. **Find the narrative hook** - why should viewers care?
4. **Collect actual assets** - logos, screenshots, product images

---

## Phase 2: Quick Clarification

Ask only essential questions (adapt based on the topic):

- **Audience**: What background to assume? (high school, undergrad, professional)
- **Length**: Short (2-5 min), medium (5-10 min), or long (10+ min)?
- **Focus**: Intuition-first or proof-heavy? Real-world applications?

---

## Phase 3: Create Scene Plan with Emotional Arc

Write a `scenes.md` file with this structure:

```markdown
# [Video Title]

## Overview
- **Topic**: [Core concept]
- **Hook**: [Opening question/mystery]
- **Target Audience**: [Prerequisites]
- **Key Insight**: [The "aha moment"]

## Narrative Arc (REQUIRED)
1. **Hook** (0-10s): Surprising visual or question
2. **Setup** (10-40s): Establish context, build curiosity
3. **Tension** (40s-1:30): Complications, accelerating pace
4. **Climax** (1:30-2:00): The "aha" - PUNCH → HOLD
5. **Resolution** (2:00-end): Implications, slower pace

## Audio/Beat Plan (REQUIRED)
- **BPM**: [number] → beat_duration = 60/BPM
- **Key sync points**:
  | Timestamp | Event | Animation | Punch Level |
  |-----------|-------|-----------|-------------|
  | 0:45 | Drop | Main reveal | PUNCH + HOLD |
  | 1:30 | Breakdown | Explanation | Slow |

---

## Scene 1: [Name]
**Duration**: ~X seconds
**Emotional Beat**: [Hook/Build/Punch/Breathe]
**Purpose**: [What this accomplishes]

### Visual Elements
- [Mobjects, animations, camera moves]
- [ACTUAL logo files to use - with URLs]

### Timing
- Animation style: [punch/reveal/buildup]
- Hold after: [X seconds]
- Camera: [static/zoom/shake]

### Content
[What happens, what's shown]

### Narration Notes
[Key points, tone, pacing]

---
[Repeat for each scene]
```

---

## Phase 4: Implement with Manim

### Project Setup

```bash
# Check if manim is installed
which manim || pip install manim

# Create project with assets directory
PROJECT="~/manim_projects/[project_name]"
mkdir -p "$PROJECT/assets"
cd "$PROJECT"

# Download Means brand assets
curl -so assets/MeansSig.png "https://c.jws.ai/MeansSigBlack.png"
curl -so assets/MeansSigWhite.png "https://c.jws.ai/MeansSigWhite.png"
curl -so assets/JWSWhite.png "https://c.jws.ai/JWS%20Icon%20Only%20White.png"
curl -so assets/JWSBlack.png "https://c.jws.ai/JWS%20Icon%20Only%20Black.png"
```

### Means Brand Colors (from means.ai CSS)

```python
# === MEANS.AI BRAND COLORS (extracted from live CSS) ===

# Primary backgrounds
MEANS_BLACK = "#000000"
MEANS_DARK = "#1F2429"
# Background is: linear-gradient(#000000, #1F2429)

# Text colors
MEANS_TEXT_DARK = "#f5f5f7"   # Light text on dark bg
MEANS_TEXT_LIGHT = "#282a2c"  # Dark text on light bg

# Accent colors
MEANS_CYAN = "#82D6FF"        # Primary accent / links
MEANS_BLUE = "#007AFF"        # Apple blue
MEANS_GREEN = "#34C759"       # Success / server
MEANS_RED = "#FF3B30"         # Error / warning
MEANS_ORANGE = "#FF9500"      # Web / highlight
MEANS_PURPLE = "#AF52DE"      # visionOS / spatial

# Glass effects
MEANS_GLASS_BG = "#1F242999"           # 60% opacity dark
MEANS_GLASS_BORDER = "#FFFFFF1A"       # 10% white border
MEANS_HEADER_BG = "#000000AD"          # 68% opacity black
```

### Complete Base Scene Template

```python
from manim import *
import numpy as np
import os

# === MEANS BRAND ===
MEANS_BLACK = "#000000"
MEANS_DARK = "#1F2429"
MEANS_TEXT = "#f5f5f7"
MEANS_CYAN = "#82D6FF"
MEANS_BLUE = "#007AFF"
MEANS_GREEN = "#34C759"
MEANS_RED = "#FF3B30"
MEANS_ORANGE = "#FF9500"
MEANS_PURPLE = "#AF52DE"

# === EMOTIONAL TIMING ===
TIMING_BUILDUP = [0.8, 0.5, 0.35, 0.25, 0.15]  # Accelerating
PUNCH = 0.12
HOLD_IMPACT = 2.5
DRAMATIC_REVEAL = 1.8
BREATHE = 1.5
BEAT = 0.5  # 120 BPM

# Safe fonts
FONT_BODY = "Helvetica Neue"
FONT_CODE = "Menlo"

# Asset path
ASSETS = os.path.join(os.path.dirname(__file__), "assets")


def create_microdot_grid(spacing=0.24, opacity=0.09):
    """Exact means.ai microdot grid: 24px spacing, 0.09 opacity white dots"""
    dots = VGroup()
    for x in np.arange(-8, 8.5, spacing):
        for y in np.arange(-5, 5.5, spacing):
            dot = Dot(point=[x, y, 0], radius=0.012, color=WHITE)
            dot.set_opacity(opacity)
            dots.add(dot)
    return dots


def create_glass_panel(width, height, color=MEANS_CYAN):
    """Glassmorphism panel matching means.ai aesthetic"""
    return RoundedRectangle(
        corner_radius=0.15,
        width=width, height=height,
        fill_color=MEANS_DARK, fill_opacity=0.7,
        stroke_color=color, stroke_width=2
    )


class MeansScene(MovingCameraScene):
    """Base scene with Means styling and emotional timing helpers"""

    def setup(self):
        super().setup()
        self.camera.background_color = MEANS_BLACK
        self.grid = create_microdot_grid()
        self.add(self.grid)

    def load_logo(self, name, scale=0.5):
        """Load a logo from assets directory"""
        path = os.path.join(ASSETS, name)
        if os.path.exists(path):
            return ImageMobject(path).scale(scale)
        return None

    # === EMOTIONAL TIMING METHODS ===

    def punch(self, mobject, scale_factor=1.15):
        """
        PUNCH animation: fast snap with hard stop, then hold.
        Use for key reveals and impact moments.
        """
        self.wait(0.3)  # Anticipation pause
        self.play(
            FadeIn(mobject, scale=scale_factor),
            run_time=PUNCH,
            rate_func=rate_functions.rush_into
        )
        self.wait(HOLD_IMPACT)

    def dramatic_reveal(self, mobject, hold=2.0):
        """Slow, deliberate reveal with proper weight"""
        self.wait(0.4)  # Anticipation
        self.play(
            FadeIn(mobject),
            run_time=DRAMATIC_REVEAL,
            rate_func=rate_functions.ease_out_expo
        )
        self.wait(hold)

    def build_tension(self, items, climax):
        """
        Accelerating sequence leading to punch.
        items: list of mobjects to show quickly
        climax: the mobject to PUNCH at the end
        """
        for i, item in enumerate(items):
            delay = TIMING_BUILDUP[min(i, len(TIMING_BUILDUP)-1)]
            self.play(FadeIn(item), run_time=delay)

        self.punch(climax)

    def camera_punch(self, target, zoom=1.3, hold=1.0):
        """Zoom punch for emphasis - snap in, hold, ease out"""
        self.play(
            self.camera.frame.animate.scale(1/zoom).move_to(target),
            run_time=PUNCH,
            rate_func=rate_functions.rush_into
        )
        self.wait(hold)
        self.play(
            self.camera.frame.animate.scale(zoom).move_to(ORIGIN),
            run_time=0.6,
            rate_func=rate_functions.ease_out_expo
        )

    def breathe(self, duration=BREATHE):
        """Pause for comprehension - use after complex reveals"""
        self.wait(duration)

    def beat_sync(self, animation, beats=1, bpm=120):
        """Play animation synced to beat grid"""
        beat_duration = 60 / bpm
        self.play(animation, run_time=beat_duration * beats)

    def stagger_in(self, items, base_delay=0.15):
        """Staggered entrance with acceleration"""
        self.play(
            LaggedStart(
                *[FadeIn(item, shift=UP*0.2) for item in items],
                lag_ratio=base_delay
            )
        )

    def construct(self):
        # Override in subclass
        pass
```

---

## Animation Patterns

### The Punch Pattern (Most Important)

```python
# DON'T: Uniform timing (boring)
self.play(FadeIn(item1), run_time=0.6)
self.play(FadeIn(item2), run_time=0.6)
self.play(FadeIn(item3), run_time=0.6)  # Flat, no impact

# DO: Build and punch (engaging)
self.play(FadeIn(item1), run_time=0.6)
self.play(FadeIn(item2), run_time=0.4)
self.play(FadeIn(item3), run_time=0.25)
self.wait(0.3)  # Anticipation
self.punch(final_item)  # SNAP + HOLD
```

### Camera Moves for Emphasis

```python
# Zoom punch on key element
self.camera_punch(important_element, zoom=1.5)

# Slow pan for exploration
self.play(
    self.camera.frame.animate.move_to(RIGHT * 3),
    run_time=2.0,
    rate_func=rate_functions.ease_in_out_sine
)
```

### Glass Panels (means.ai style)

```python
panel = create_glass_panel(4, 2, MEANS_CYAN)
label = Text("Module", font=FONT_CODE, font_size=20, color=MEANS_CYAN)
label.move_to(panel.get_center())

# Use dramatic_reveal for important panels
self.dramatic_reveal(VGroup(panel, label))
```

### Architecture Diagrams

```python
def create_arch_box(label, color, width=2.5, height=1.2):
    """Create labeled architecture box"""
    box = create_glass_panel(width, height, color)
    text = Text(label, font=FONT_CODE, font_size=18, color=color)
    text.move_to(box.get_center())
    return VGroup(box, text)

# Build architecture with tension
server = create_arch_box("JWS Server", MEANS_GREEN)
client = create_arch_box("JCS Client", MEANS_BLUE)
bridge = create_arch_box("Bridge", MEANS_ORANGE)

server.move_to(UP * 2)
client.move_to(DOWN * 2)
bridge.move_to(ORIGIN)

# Tension build: show parts, then PUNCH the connection
self.build_tension(
    [server, client],
    bridge  # This gets the PUNCH
)

# Draw connections with drama
arrow1 = Arrow(server.get_bottom(), bridge.get_top(), color=MEANS_CYAN)
arrow2 = Arrow(bridge.get_bottom(), client.get_top(), color=MEANS_CYAN)
self.play(
    Create(arrow1), Create(arrow2),
    run_time=0.3,
    rate_func=rate_functions.rush_into
)
self.breathe()
```

### Platform Grid with Stagger

```python
platforms = ["iOS", "macOS", "visionOS", "Web", "Linux", "tvOS"]
colors = [MEANS_BLUE, MEANS_GREEN, MEANS_PURPLE, MEANS_ORANGE, MEANS_RED, MEANS_CYAN]

cards = VGroup()
for name, color in zip(platforms, colors):
    card = create_glass_panel(1.5, 0.7, color)
    txt = Text(name, font=FONT_CODE, font_size=16, color=color)
    txt.move_to(card.get_center())
    cards.add(VGroup(card, txt))

cards.arrange_in_grid(rows=2, buff=0.3)

# Staggered entrance with acceleration
self.stagger_in(cards, base_delay=0.1)
self.breathe()
```

### Code Reveal with Typewriter Effect

```python
code_lines = [
    "protocol JCSAuthCore {",
    "    func login() async throws",
    "    func greet() async throws",
    "}"
]

code_group = VGroup()
for line in code_lines:
    txt = Text(line, font=FONT_CODE, font_size=20, color=MEANS_CYAN)
    code_group.add(txt)

code_group.arrange(DOWN, aligned_edge=LEFT, buff=0.15)

# Accelerating reveal
delays = [0.5, 0.35, 0.25, 0.15]
for i, line in enumerate(code_group):
    self.play(
        FadeIn(line, shift=RIGHT*0.3),
        run_time=delays[min(i, len(delays)-1)]
    )

self.breathe()
```

---

## Phase 5: Render

```bash
# Preview (low quality, fast)
manim -pql scene.py SceneName

# Medium quality (720p30)
manim -qm scene.py SceneName

# High quality (1080p60)
manim -pqh scene.py SceneName

# 4K
manim -pqk scene.py SceneName

# GIF output
manim -qm --format gif scene.py SceneName

# Combine multiple scenes with ffmpeg
cd media/videos/*/720p30/
cat > files.txt << EOF
file 'Scene01.mp4'
file 'Scene02.mp4'
file 'Scene03.mp4'
EOF
ffmpeg -f concat -safe 0 -i files.txt -c copy ../Combined.mp4
```

---

## 3b1b Style Principles (Updated)

### Visual Storytelling
- **Show, don't tell** - every concept needs a visual
- **Progressive revelation** - build complexity gradually
- **Visual continuity** - transform objects, don't replace
- **Use actual assets** - real logos, screenshots, not approximations

### Emotional Pacing (NOT Fibonacci)
- **Buildup**: Accelerating sequence (0.8 → 0.5 → 0.3 → 0.15s)
- **Punch**: Snap in fast (0.12s) with rush_into
- **Hold**: Let impact land (2-3s silence)
- **Breathe**: Pause for comprehension (1.5s)
- **Contrast**: Fast sections need slow sections

### Engagement
- **Pose questions** - make viewers curious first
- **Build tension** - accelerate toward key moments
- **Punch the insight** - make "aha" moments SNAP
- **Hold for impact** - silence after reveals
- **Breathe** - give viewers time to process

---

## Scene Structure Template (REQUIRED)

Every video must follow this emotional arc:

```
0:00-0:10  HOOK         Fast cuts, surprising visual, question
0:10-0:40  SETUP        Establish context, steady rhythm
0:40-1:30  TENSION      Accelerating complexity, building
1:30-2:00  CLIMAX       PUNCH → HOLD → key insight lands
2:00-end   RESOLUTION   Slower, implications, closing
```

---

## Execution Flow

1. **Spawn parallel agents** to research topic deeply (codebase + web)
2. **Download actual assets** (logos, images) to assets/ directory
3. **Ask** 2-3 clarifying questions max
4. **Write** `scenes.md` with emotional arc and beat sync plan
5. **Create** Python files using MeansScene base class
6. **Use punch/build_tension/breathe** methods for pacing
7. **Render** preview, iterate, then final quality
8. **Combine** scenes with ffmpeg if needed
9. **Report** file locations and how to view

---

## Output Structure

```
~/manim_projects/[project_name]/
├── assets/
│   ├── MeansSig.png          # Downloaded logos
│   ├── JWSWhite.png
│   └── [other images]
├── scenes.md                  # Scene plan with emotional arc
├── main.py                    # All scenes
└── media/videos/
    └── main/
        ├── 480p15/           # Preview
        ├── 720p30/           # Medium
        ├── 1080p60/          # High
        └── Combined.mp4      # Final video
```

---

## Quick Reference

| Timing | Value | Usage |
|--------|-------|-------|
| Punch | 0.12s | Key reveals (rush_into) |
| Hold | 2.5s | After punch |
| Breathe | 1.5s | After complex content |
| Buildup | 0.8→0.15s | Accelerating sequence |
| Beat | 0.5s | 120 BPM sync |

| Means Colors | Hex | Usage |
|--------------|-----|-------|
| Background | `#000000` | Primary bg |
| Dark | `#1F2429` | Gradient/panels |
| Text | `#f5f5f7` | Primary text |
| Cyan | `#82D6FF` | Primary accent |
| Blue | `#007AFF` | Apple/iOS |
| Green | `#34C759` | Server/success |
| Orange | `#FF9500` | Web/highlight |
| Purple | `#AF52DE` | visionOS/spatial |
| Red | `#FF3B30` | Error/warning |

| Logo | CDN URL |
|------|---------|
| Means (black) | `https://c.jws.ai/MeansSigBlack.png` |
| Means (white) | `https://c.jws.ai/MeansSigWhite.png` |
| JWS (white) | `https://c.jws.ai/JWS%20Icon%20Only%20White.png` |

| Safe Fonts | Usage |
|------------|-------|
| Helvetica Neue | Body text |
| Menlo | Code/monospace |

| Render | Command |
|--------|---------|
| Preview | `manim -pql file.py Scene` |
| Medium | `manim -qm file.py Scene` |
| High | `manim -pqh file.py Scene` |
| 4K | `manim -pqk file.py Scene` |
