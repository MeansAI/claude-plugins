---
description: Summarize work and generate a detailed Manim video + ElevenLabs narration script
argument-hint: <timeframe: "today", "this week", "yesterday", "last 3 days", etc.>
allowed-tools: Bash, Read, Write, Edit, Grep, Glob, WebSearch, WebFetch, Task
---

# What We Did - Engineering Summary Video Generator

**Timeframe:** $ARGUMENTS (default: "today")

Generate a comprehensive technical presentation video summarizing ALL work accomplished in the specified timeframe.

**Deliverables:**
1. 40-60+ scene Manim video with rich technical graphics
2. ElevenLabs-ready narration script with Texas cowboy auctioneer voice
3. Video opened in QuickTime Player at 2x speed
4. Narration copied to clipboard

---

## Phase 0: Parse Timeframe & Gather Work

### Determine Date Range

Parse `$ARGUMENTS` to set the search window:

| Input | `--since` flag | Description |
|-------|---------------|-------------|
| `today` | `midnight` | Today only |
| `yesterday` | `yesterday.midnight` | Yesterday only |
| `this week` | `1 week ago` | Past 7 days |
| `last 3 days` | `3 days ago` | Past 3 days |
| `this month` | `1 month ago` | Past 30 days |
| _(empty)_ | `midnight` | Default: today |

### Launch Parallel Research Agents

Spawn these Task agents IN PARALLEL to gather all data:

**Agent 1: Git Commits**
```
Task(subagent_type="Explore"):
  Search for git commits across ALL repos in ~/Documents/JMLLC/
  For each repo with commits in the date range:
    - git log --since="<DATE>" --all --stat
    - Extract: commit hashes, messages, files changed, insertions/deletions
    - Check for merged PRs: gh pr list --state merged --search "merged:>DATE"
```

**Agent 2: Session Transcripts**
```
Task(subagent_type="general-purpose"):
  Find Claude session transcripts modified in the date range:
    ~/.claude/projects/-Users-justinmeans-Documents-JMLLC/*.jsonl
  For each session:
    - Extract user messages (type == "user")
    - Extract files edited (tool == "Write" or tool == "Edit")
    - Count edit operations per file
    - Identify project themes from file paths
  Return: session summaries, file edit counts, project breakdown
```

**Agent 3: Brand Assets + Memory**
```
Download brand assets to project directory:
  curl -so assets/MeansSig.png "https://c.jws.ai/MeansSigBlack.png"
  curl -so assets/MeansSigWhite.png "https://c.jws.ai/MeansSigWhite.png"
  curl -so assets/JWSWhite.png "https://c.jws.ai/JWS%20Icon%20Only%20White.png"
  curl -so assets/JWSBlack.png "https://c.jws.ai/JWS%20Icon%20Only%20Black.png"

Read MEMORY.md:
  ~/.claude/projects/-Users-justinmeans-Documents-JMLLC/memory/MEMORY.md
```

### Session Transcript Extraction

Session files are JSONL. Extract data with:
```python
import json
for line in open(path):
    msg = json.loads(line)
    if msg.get("type") == "user":
        # User message content
    if msg.get("type") == "tool_use":
        if msg["name"] in ("Write", "Edit"):
            # File path from msg["input"]["file_path"]
```

**Important:** Session files can be 10-40MB. Use parallel sub-agents to extract large sessions. Parse `type == "user"` (not "human").

---

## Phase 1: Scene Plan

Write `~/manim_projects/<PROJECT_NAME>/scenes.md` with:

1. **Overview**: Timeframe, projects touched, total stats
2. **Narrative Arc**: Hook -> Overview -> Deep Dives -> Recap
3. **Scene-by-scene plan**: 40-60+ scenes with descriptions

### Section Structure Per Project

For each project with significant work:
- Section title scene (with section number badge)
- Architecture/overview scene
- 2-5 technical detail scenes (code, pipelines, before/after)
- Stats/metrics scene

### Section Ordering

Order sections by viewer interest. Put fresh/novel work first, well-known work later. Ask if unclear.

---

## Phase 2: Write Manim Code

### CRITICAL: Use Parallel Agents for Code Generation

Split scenes across 2-3 parallel Task agents. Each agent writes a separate `main_partN.py` file.

**IMPORTANT COMPATIBILITY RULES:**
- Part 1 defines the `MeansScene` base class and ALL shared helpers
- Parts 2+ import via `from main_part1 import *`
- Parts 2+ define standalone helper functions (NOT methods) for their scenes
- ALL parts must use the SAME method signatures from MeansScene

### Project Setup

```bash
PROJ_NAME="summary_$(date +%Y%m%d)"
PROJECT="$HOME/manim_projects/$PROJ_NAME"
mkdir -p "$PROJECT/assets"
cd "$PROJECT"
```

### MeansScene Base Class (in main_part1.py)

```python
from manim import *
import numpy as np
import os

# === MEANS BRAND COLORS ===
MEANS_BG = "#0A0A0F"
MEANS_TEXT = "#E8E8F0"
MEANS_CYAN = "#00D4FF"
MEANS_RED = "#FF3366"
MEANS_GREEN = "#00FF88"
MEANS_ORANGE = "#FF8800"
MEANS_PURPLE = "#AA44FF"
MEANS_YELLOW = "#FFD700"
MEANS_BLUE = "#4488FF"
MEANS_PINK = "#FF66AA"
MEANS_GRAY = "#888899"
MEANS_DIM = "#333344"

# CRITICAL: Use macOS-safe fonts. SF Pro / SF Mono cause broken spacing in Manim.
FONT_MAIN = "Helvetica Neue"
FONT_CODE = "Menlo"

ASSETS = os.path.join(os.path.dirname(os.path.abspath(__file__)), "assets")

# Animation timing constants
PUNCH = 0.12
HOLD = 2.0
BREATHE = 1.5
BUILDUP = [0.8, 0.5, 0.35, 0.25, 0.15]


class MeansScene(Scene):
    def setup(self):
        self.camera.background_color = MEANS_BG

    def means_title(self, text, color=MEANS_CYAN, font_size=48):
        return Text(text, font=FONT_MAIN, font_size=font_size, color=color, weight=BOLD)

    def means_body(self, text, font_size=24, color=MEANS_TEXT):
        return Text(text, font=FONT_MAIN, font_size=font_size, color=color)

    def means_code(self, text, font_size=18, color=MEANS_TEXT):
        return Text(text, font=FONT_CODE, font_size=font_size, color=color)

    def glass_panel(self, w, h, color=MEANS_CYAN, fill_opacity=0.08, stroke_opacity=0.4):
        return RoundedRectangle(
            corner_radius=0.15, width=w, height=h,
            fill_color=color, fill_opacity=fill_opacity,
            stroke_color=color, stroke_width=1.5, stroke_opacity=stroke_opacity,
        )

    def section_header(self, title, subtitle="", color=MEANS_CYAN):
        group = VGroup()
        line_top = Line(LEFT * 5, RIGHT * 5, color=color, stroke_width=2, stroke_opacity=0.6)
        t = self.means_title(title, color=color, font_size=42)
        line_bot = Line(LEFT * 5, RIGHT * 5, color=color, stroke_width=2, stroke_opacity=0.6)
        group.add(line_top, t, line_bot)
        if subtitle:
            sub = self.means_body(subtitle, font_size=20, color=MEANS_GRAY)
            group.add(sub)
        group.arrange(DOWN, buff=0.3)
        return group

    def stat_counter(self, label, value, color=MEANS_CYAN):
        val = Text(str(value), font=FONT_MAIN, font_size=56, color=color, weight=BOLD)
        lab = Text(label, font=FONT_MAIN, font_size=18, color=MEANS_GRAY)
        return VGroup(val, lab).arrange(DOWN, buff=0.15)

    def bullet_list(self, items, color=MEANS_TEXT, font_size=20, bullet_color=MEANS_CYAN):
        group = VGroup()
        for item in items:
            bullet = Text("*", font=FONT_CODE, font_size=font_size, color=bullet_color)
            text = Text(item, font=FONT_MAIN, font_size=font_size, color=color)
            row = VGroup(bullet, text).arrange(RIGHT, buff=0.2)
            group.add(row)
        group.arrange(DOWN, buff=0.25, aligned_edge=LEFT)
        return group

    def code_block(self, code_text, color=MEANS_CYAN, font_size=16):
        lines = code_text.split("\n")
        panel = self.glass_panel(10, len(lines) * 0.4 + 0.6, color=color, fill_opacity=0.05)
        code = Text(code_text, font=FONT_CODE, font_size=font_size, color=MEANS_TEXT)
        return VGroup(panel, code)

    def flow_arrow(self, start, end, color=MEANS_CYAN):
        return Arrow(start, end, color=color, stroke_width=2, tip_length=0.2, buff=0.1)

    def fade_all(self, rt=0.5):
        self.play(FadeOut(Group(*self.mobjects), lag_ratio=0.02), run_time=rt)

    def punch_in(self, mob, scale=1.15):
        self.wait(0.25)
        self.play(FadeIn(mob, scale=scale), run_time=PUNCH, rate_func=rush_into)
        self.wait(HOLD)
```

### Standalone Helpers for Part 2+ Files

Parts 2+ should define standalone functions AFTER `from main_part1 import *`:

```python
from main_part1 import *

# Standalone helpers (different signatures from MeansScene methods)
def glass_panel(w, h, color=MEANS_CYAN, opacity=0.85):
    return RoundedRectangle(
        corner_radius=0.15, width=w, height=h,
        fill_color=color, fill_opacity=opacity,
        stroke_color=color, stroke_width=2,
    )

def section_header(label, color=MEANS_CYAN, font_size=40):
    header = Text(label, font=FONT_CODE, font_size=font_size, color=color, weight=BOLD)
    line = Line(LEFT * 4.5, RIGHT * 4.5, color=color, stroke_width=2)
    line.next_to(header, DOWN, buff=0.2)
    return VGroup(header, line)

def subtitle_text(text, font_size=22, color=MEANS_TEXT):
    return Text(text, font=FONT_MAIN, font_size=font_size, color=color)

def stat_counter(label, value, color=MEANS_CYAN, font_size=48):
    val = Text(str(value), font=FONT_CODE, font_size=font_size, color=color, weight=BOLD)
    lbl = Text(label, font=FONT_MAIN, font_size=20, color=MEANS_TEXT)
    lbl.next_to(val, DOWN, buff=0.15)
    return VGroup(val, lbl)

def bullet_list(items, font_size=18, color=MEANS_TEXT, bullet_color=None):
    rows = VGroup()
    for text in items:
        bc = bullet_color if bullet_color else color
        dot = Dot(radius=0.04, color=bc)
        label = Text(text, font=FONT_MAIN, font_size=font_size, color=color)
        label.next_to(dot, RIGHT, buff=0.15)
        rows.add(VGroup(dot, label))
    rows.arrange(DOWN, aligned_edge=LEFT, buff=0.22)
    return rows

def labeled_box(label, color, w=2.5, h=1.0, font_size=14):
    box = glass_panel(w, h, color)
    txt = Text(label, font=FONT_CODE, font_size=font_size, color=color)
    txt.move_to(box.get_center())
    return VGroup(box, txt)
```

### Scene Naming Convention

```python
class S00_Title(MeansScene): ...      # Opening
class S01_Hook(MeansScene): ...       # Stats counters
class S02_ProjectMap(MeansScene): ... # Hex grid overview

class S10_ProjectTitle(MeansScene): ...  # Section 2 title
class S11_Detail(MeansScene): ...        # Detail scenes
# ...

class S90_Summary(MeansScene): ...    # Recap grid
class S91_Stats(MeansScene): ...      # Grand finale stats
class S92_TechStack(MeansScene): ...  # Tech badges
class S93_Closing(MeansScene): ...    # Logo + tagline
```

### Gotchas

- **numpy `.normalized()` does NOT exist** — use `v / np.linalg.norm(v)` instead
- **Always use `-a` flag** when rendering: `manim -qm -a main_partN.py`
- **Never use `from manim import *` in try/except fallbacks** — leads to duplicate MeansScene classes
- **Delete render cache** when changing fonts: `rm -rf media/`
- **Section title scenes** should have a large faded section number badge

---

## Phase 3: Write Narration Script

Write TWO narration files:

1. `narration_script.txt` — timestamped with scene markers
2. `narration_elevenlabs.txt` — clipboard-ready, no timestamps

### Voice Style Tags

| Tag | Usage |
|-----|-------|
| `[smooth raspy drawl with rhythm]` | Explanations, flow |
| `[auctioneer pace FAST rapid-fire quantitative]` | Stats, lists, code |
| `[drunk humorous cowboy]` | Intros, transitions |
| `[dramatic] [slow]` | Big reveals |
| `[excited] [fast]` | Wins, breakthroughs |
| `[yells]` | Emphasis |
| `[laughs]` | After wins |

---

## Phase 4: Render, Combine, Deliver

### Render All Parts in Parallel

```bash
cd ~/manim_projects/<PROJECT>
# Delete stale cache if fonts or base class changed
rm -rf media/
# Render all parts simultaneously
manim -qm -a main_part1.py &
manim -qm -a main_part2.py &
manim -qm -a main_part3.py &
wait
```

### Create Ordered Concat List

Write `concat_list.txt` with scene files in presentation order. This is where section reordering happens — the concat list controls final order regardless of scene class numbering.

```bash
# Example:
# file 'media/videos/main_part1/720p30/S00_Title.mp4'
# file 'media/videos/main_part1/720p30/S01_Hook.mp4'
# ...sections in desired order...
# file 'media/videos/main_part3/720p30/S93_Closing.mp4'
```

### Combine and Speed Up

```bash
# Combine scenes
ffmpeg -y -f concat -safe 0 -i concat_list.txt -c copy combined_raw.mp4

# Speed up to 2x
ffmpeg -y -i combined_raw.mp4 -filter:v "setpts=0.5*PTS" -an final.mp4
```

### Deliver

```bash
# Open in QuickTime
open -a "QuickTime Player" final.mp4

# Copy narration to clipboard
cat narration_elevenlabs.txt | pbcopy
echo "Narration copied to clipboard"
```

---

## Output Structure

```
~/manim_projects/<PROJECT>/
├── assets/
│   ├── MeansSig.png
│   ├── MeansSigWhite.png
│   ├── JWSWhite.png
│   └── JWSBlack.png
├── scenes.md
├── main_part1.py          # Base class + first batch of scenes
├── main_part2.py          # Middle scenes
├── main_part3.py          # Final scenes + closing
├── concat_list.txt        # Ordered scene list
├── narration_script.txt   # Timestamped narration
├── narration_elevenlabs.txt  # Clipboard-ready
├── combined_raw.mp4       # 1x speed
└── final.mp4              # 2x speed (delivered)
```

---

## Final Deliverables Checklist

1. **Video**: `final.mp4` opened in QuickTime Player (2x speed)
2. **Narration**: Copied to clipboard (paste into ElevenLabs)
3. **Report**: Quick summary of scenes, duration, projects covered
