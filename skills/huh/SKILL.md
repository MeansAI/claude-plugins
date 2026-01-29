---
description: Visual Explainer - Transform confusing concepts into interactive web dashboards with charts, diagrams, and clear breakdowns
argument-hint: <complex topic, concept, or data to visualize>
allowed-tools: Bash, Read, Write, Edit, Grep, Glob, Task, WebSearch, WebFetch
---

# /huh - Visual Explainer Dashboard

**Input:** $ARGUMENTS

Transform any confusing concept, complex data, or abstract idea into an interactive, visual web dashboard that makes it instantly understandable. Uses the Means glassmorphism design system.

---

## How It Works

When the user says `/huh [something complex]`, create a standalone HTML dashboard that explains it visually using:
- Interactive charts (Chart.js)
- Glassmorphic cards with key stats
- Visual hierarchies and relationships
- Progress indicators
- Icon-enhanced breakdowns
- Dark theme for focus

---

## Phase 1: Understand What's Confusing

Before building, identify:
1. **What is the user struggling to understand?** (concept, data, system, process?)
2. **What's the core insight?** (the "aha moment" they need)
3. **What comparisons help?** (analogies, relative scales, before/after)
4. **What structure fits?** (timeline, hierarchy, flow, comparison, metrics)

---

## Phase 2: Choose Dashboard Layout

Select the appropriate visualization pattern:

| Pattern | Best For | Example |
|---------|----------|---------|
| **Metrics Dashboard** | Numbers, KPIs, statistics | Financial data, analytics |
| **Process Flow** | Steps, workflows, how-things-work | Algorithms, business processes |
| **Comparison Grid** | Options, tradeoffs, versus | Pricing plans, tech stacks |
| **Timeline** | History, phases, roadmaps | Project plans, evolution |
| **Hierarchy/Tree** | Organizations, dependencies | Org charts, file systems |
| **Relationship Map** | Connections, flows, networks | APIs, system architecture |

---

## Phase 3: Build the Dashboard

### Base Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[TOPIC] Explained</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap');
        * { font-family: 'Inter', sans-serif; }
        body { background: #0f0f1a; color: #fff; min-height: 100vh; }
        .glass { background: rgba(255,255,255,0.05); backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.1); }
        .gradient-text { background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
        .glow { box-shadow: 0 0 40px rgba(102, 126, 234, 0.3); }
        .stat-card { transition: transform 0.2s, box-shadow 0.2s; }
        .stat-card:hover { transform: translateY(-4px); box-shadow: 0 20px 40px rgba(0,0,0,0.3); }
        .insight-card { border-left: 4px solid #667eea; }
    </style>
</head>
<body class="p-8">
    <div class="max-w-6xl mx-auto">
        <!-- Header -->
        <div class="mb-8">
            <h1 class="text-4xl font-black gradient-text">[TOPIC] Explained</h1>
            <p class="text-gray-400 mt-2">[One-line summary of what this dashboard shows]</p>
        </div>

        <!-- Key Takeaways (always first) -->
        <div class="glass rounded-2xl p-6 mb-8 glow">
            <h2 class="text-xl font-bold mb-4"><i class="fas fa-lightbulb text-yellow-400 mr-2"></i>TL;DR - The Main Point</h2>
            <p class="text-lg">[Core insight in one clear sentence]</p>
        </div>

        <!-- Main Content Grid -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-8">
            <!-- [Add stat cards, charts, visualizations here] -->
        </div>

        <!-- Detailed Breakdown -->
        <div class="glass rounded-2xl p-6">
            <h2 class="text-xl font-bold mb-4"><i class="fas fa-list mr-2"></i>The Breakdown</h2>
            <!-- [Add detailed explanation sections] -->
        </div>
    </div>

    <script>
        // [Add Chart.js or interactive JavaScript here]
    </script>
</body>
</html>
```

### Design System Reference

**Colors:**
```css
/* Backgrounds */
--bg-dark: #0f0f1a;
--glass-bg: rgba(255,255,255,0.05);
--glass-border: rgba(255,255,255,0.1);

/* Accent Colors */
--accent-purple: #667eea;
--accent-pink: #764ba2;
--accent-magenta: #f093fb;

/* Status Colors */
--green: #10b981;  /* Success, positive, done */
--blue: #3b82f6;   /* Info, primary action */
--yellow: #f59e0b; /* Warning, attention */
--red: #ef4444;    /* Error, negative, blocked */
--purple: #8b5cf6; /* Special, premium */
--orange: #f97316; /* Highlight, trending */
```

**Stat Card Pattern:**
```html
<div class="stat-card glass rounded-2xl p-6">
    <div class="flex justify-between items-start">
        <div>
            <p class="text-gray-400 text-sm">[Label]</p>
            <p class="text-4xl font-black mt-2">[Big Number/Value]</p>
            <p class="text-[color]-400 text-sm mt-1"><i class="fas fa-[icon]"></i> [Context]</p>
        </div>
        <div class="w-12 h-12 rounded-xl bg-[color]-500/20 flex items-center justify-center">
            <i class="fas fa-[icon] text-[color]-400 text-xl"></i>
        </div>
    </div>
</div>
```

**Comparison Cards:**
```html
<div class="bg-[color]-500/10 border border-[color]-500/30 rounded-xl p-4">
    <h4 class="font-semibold text-[color]-400 mb-2"><i class="fas fa-[icon] mr-2"></i>[Title]</h4>
    <ul class="text-sm text-gray-300 space-y-1">
        <li>[Point 1]</li>
        <li>[Point 2]</li>
    </ul>
</div>
```

**Chart.js Quick Reference:**
```javascript
// Bar Chart
new Chart(document.getElementById('myChart'), {
    type: 'bar',
    data: {
        labels: ['A', 'B', 'C'],
        datasets: [{
            label: 'Data',
            data: [10, 20, 30],
            backgroundColor: ['#667eea', '#764ba2', '#f093fb']
        }]
    },
    options: {
        responsive: true,
        plugins: { legend: { labels: { color: '#fff' } } },
        scales: {
            y: { ticks: { color: '#9ca3af' }, grid: { color: 'rgba(255,255,255,0.1)' } },
            x: { ticks: { color: '#9ca3af' }, grid: { display: false } }
        }
    }
});

// Doughnut Chart (for proportions)
type: 'doughnut'

// Line Chart (for trends)
type: 'line'
```

---

## Phase 4: Output & Display

1. **Save the dashboard** to a sensible location:
   - If in a project: `./[topic]-explained.html`
   - Default: `~/Documents/projects/explainers/[topic]-explained.html`

2. **Open in browser** automatically:
   ```bash
   open [filepath]
   ```

3. **Tell the user** what you created and where it's saved.

---

## Examples

**User:** `/huh how does EMDR therapy work`
- Creates visual flow of EMDR process
- Timeline of a typical session
- Before/after brain state diagrams
- Stats on effectiveness

**User:** `/huh the difference between LLCs and S-Corps`
- Side-by-side comparison cards
- Flowchart for choosing
- Tax implications visualized
- Pros/cons breakdown

**User:** `/huh what's eating my AWS bill`
- Cost breakdown pie chart
- Service-by-service comparison
- Trend line of spending
- Optimization recommendations

**User:** `/huh git rebase vs merge`
- Visual tree diagrams
- Side-by-side command comparison
- When-to-use decision flow
- History visualization

---

## Best Practices

1. **Lead with the answer** - Put the main insight at the top
2. **Use numbers** - Quantify wherever possible
3. **Visual hierarchy** - Most important = biggest/first
4. **Reduce cognitive load** - Max 7 items per section
5. **Color coding** - Consistent meaning (green=good, red=bad, etc.)
6. **Icons for scanning** - Every section gets an icon
7. **Mobile responsive** - Use Tailwind's responsive classes

---

## Quick Reference: Icon Library

| Category | Icons |
|----------|-------|
| **Status** | `fa-check-circle`, `fa-times-circle`, `fa-exclamation-triangle`, `fa-info-circle` |
| **Actions** | `fa-rocket`, `fa-bolt`, `fa-play`, `fa-cog` |
| **Data** | `fa-chart-line`, `fa-chart-bar`, `fa-chart-pie`, `fa-database` |
| **Time** | `fa-clock`, `fa-calendar`, `fa-history`, `fa-hourglass` |
| **Money** | `fa-dollar-sign`, `fa-credit-card`, `fa-wallet`, `fa-piggy-bank` |
| **People** | `fa-user`, `fa-users`, `fa-user-check`, `fa-handshake` |
| **Objects** | `fa-file`, `fa-folder`, `fa-box`, `fa-cube` |
| **Concepts** | `fa-lightbulb`, `fa-brain`, `fa-puzzle-piece`, `fa-magic` |

---

## Execution Checklist

- [ ] Read and understand the complex topic
- [ ] Identify the core insight ("aha moment")
- [ ] Choose appropriate visualization pattern
- [ ] Build dashboard with glassmorphism styling
- [ ] Include TL;DR at top
- [ ] Add interactive charts if data is involved
- [ ] Save to appropriate location
- [ ] Open in browser for user
- [ ] Confirm location and offer to iterate
