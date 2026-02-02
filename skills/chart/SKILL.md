---
name: chart
description: Generate minimal, clean charts using QuickChart.io with shadcn-style grayscale design. Supports bar, line, area, pie, and more. No dependencies required.
allowed-tools:
  - Bash(curl *)
  - Read
  - Write
---

# Chart Generation Skill

Generate clean, minimal charts with shadcn-inspired grayscale styling using QuickChart.io (no dependencies needed).

## Quick Usage

```
/chart bar chart: Category A 45, Category B 32, Category C 78
/chart line chart: Jan 100, Feb 150, Mar 200, Apr 180
/chart area chart: Q1 100, Q2 150, Q3 200, Q4 180
/chart pie chart: Sales 45%, Marketing 30%, Engineering 25%
```

## Data Input Formats

### Inline (Simple)
```
Label1 Value1, Label2 Value2, Label3 Value3
```

### From file
```
/chart line chart from /path/to/data.csv
```

## Chart Types

| Type | Keywords | Description |
|------|----------|-------------|
| Bar (vertical) | `bar`, `bar chart` | Vertical bars |
| Bar (horizontal) | `horizontal bar`, `hbar` | Horizontal bars |
| Line | `line`, `line chart` | Time series, trends |
| Area | `area`, `area chart` | Filled line chart |
| Pie | `pie`, `pie chart` | Proportions |
| Doughnut | `doughnut`, `donut` | Ring-style proportions |

## Options

- **title**: `title "My Chart Title"`
- **output**: `save to /path/to/chart.png` (default: `~/Downloads/chart_YYYYMMDD_HHMMSS.png`)
- **size**: `size 800x600` (width x height in pixels, default: 600x400)

## Color Palette (Grayscale - shadcn/Zinc)

| Color | Hex | Usage |
|-------|-----|-------|
| zinc-900 | `#18181B` | Primary series |
| zinc-700 | `#3F3F46` | Secondary series |
| zinc-500 | `#71717A` | Third series |
| zinc-400 | `#A1A1AA` | Fourth series |
| zinc-300 | `#D4D4D8` | Fifth series |
| zinc-200 | `#E4E4E7` | Sixth series |
| zinc-100 | `#F4F4F5` | Grid lines |

## Style Elements

- Clean, minimal design
- Grayscale color palette
- Subtle grid lines (zinc-100)
- Rounded corners on bars (radius: 4)
- Smooth curves on lines (tension: 0.3)
- No borders, light aesthetic
- Semi-transparent fills for area charts

## Instructions

When the user requests a chart:

1. **Parse the request** to identify:
   - Chart type (bar, line, area, pie, doughnut, horizontal bar)
   - Data (inline or file path)
   - Title (if provided)
   - Output path (default: `~/Downloads/chart_$(date +%Y%m%d_%H%M%S).png`)
   - Size (default: 600x400)

2. **Build the Chart.js configuration** using this shadcn-style template:

```json
{
  "type": "bar",
  "data": {
    "labels": ["A", "B", "C"],
    "datasets": [{
      "data": [10, 20, 30],
      "backgroundColor": "#18181B",
      "borderRadius": 4
    }]
  },
  "options": {
    "layout": {
      "padding": { "top": 20, "right": 30, "bottom": 20, "left": 20 }
    },
    "plugins": {
      "title": {
        "display": true,
        "text": "Chart Title",
        "align": "start",
        "font": { "size": 16, "weight": "600", "family": "Inter, system-ui, sans-serif" },
        "color": "#18181B",
        "padding": { "bottom": 20 }
      },
      "legend": { "display": false },
      "datalabels": { "display": false }
    },
    "scales": {
      "y": {
        "beginAtZero": true,
        "border": { "display": false },
        "grid": { "color": "#F4F4F5" },
        "ticks": { "color": "#71717A", "padding": 10, "font": { "size": 11 } }
      },
      "x": {
        "border": { "display": false },
        "grid": { "display": false },
        "ticks": { "color": "#71717A", "padding": 10, "font": { "size": 11 } }
      }
    }
  }
}
```

3. **Generate the chart** using POST to QuickChart.io:

```bash
curl -X POST https://quickchart.io/chart \
  -H 'Content-Type: application/json' \
  -d '{
    "version": "4",
    "backgroundColor": "white",
    "width": 600,
    "height": 400,
    "chart": CHART_CONFIG_JSON
  }' \
  --output ~/Downloads/chart_$(date +%Y%m%d_%H%M%S).png
```

4. **Show the result** by reading the generated image file with the Read tool

## Chart Type Configurations

### Bar Chart
```json
{
  "type": "bar",
  "data": {
    "labels": ["A", "B", "C"],
    "datasets": [{
      "data": [10, 20, 30],
      "backgroundColor": "#18181B",
      "borderRadius": 4
    }]
  }
}
```

### Horizontal Bar Chart
```json
{
  "type": "bar",
  "data": {
    "labels": ["A", "B", "C"],
    "datasets": [{
      "data": [10, 20, 30],
      "backgroundColor": "#18181B",
      "borderRadius": 4
    }]
  },
  "options": {
    "indexAxis": "y"
  }
}
```

### Line Chart
```json
{
  "type": "line",
  "data": {
    "labels": ["Jan", "Feb", "Mar"],
    "datasets": [{
      "data": [10, 20, 30],
      "borderColor": "#18181B",
      "borderWidth": 2,
      "tension": 0.3,
      "pointRadius": 0,
      "fill": false
    }]
  }
}
```

### Area Chart (Filled Line)
```json
{
  "type": "line",
  "data": {
    "labels": ["Jan", "Feb", "Mar"],
    "datasets": [{
      "data": [10, 20, 30],
      "borderColor": "#18181B",
      "backgroundColor": "rgba(24, 24, 27, 0.1)",
      "borderWidth": 2,
      "tension": 0.3,
      "pointRadius": 0,
      "fill": true
    }]
  }
}
```

### Multi-Series Area Chart
```json
{
  "type": "line",
  "data": {
    "labels": ["Jan", "Feb", "Mar", "Apr"],
    "datasets": [
      {
        "label": "Desktop",
        "data": [100, 150, 120, 180],
        "borderColor": "#18181B",
        "backgroundColor": "rgba(24, 24, 27, 0.15)",
        "borderWidth": 2,
        "tension": 0.3,
        "fill": true
      },
      {
        "label": "Mobile",
        "data": [80, 120, 140, 160],
        "borderColor": "#71717A",
        "backgroundColor": "rgba(113, 113, 122, 0.15)",
        "borderWidth": 2,
        "tension": 0.3,
        "fill": true
      }
    ]
  },
  "options": {
    "plugins": {
      "legend": { "display": true, "position": "bottom", "labels": { "color": "#71717A", "usePointStyle": true } }
    }
  }
}
```

### Pie / Doughnut Chart
```json
{
  "type": "doughnut",
  "data": {
    "labels": ["A", "B", "C"],
    "datasets": [{
      "data": [45, 30, 25],
      "backgroundColor": ["#18181B", "#71717A", "#D4D4D8"],
      "borderWidth": 0
    }]
  },
  "options": {
    "cutout": "60%"
  }
}
```

## Examples

### Simple Bar Chart
```
/chart bar chart: Revenue 450, Costs 320, Profit 130 title "Q4 Financial Summary"
```

Generated curl:
```bash
curl -X POST https://quickchart.io/chart \
  -H 'Content-Type: application/json' \
  -d '{
    "version": "4",
    "backgroundColor": "white",
    "width": 600,
    "height": 400,
    "chart": {
      "type": "bar",
      "data": {
        "labels": ["Revenue", "Costs", "Profit"],
        "datasets": [{"data": [450, 320, 130], "backgroundColor": "#18181B", "borderRadius": 4}]
      },
      "options": {
        "layout": {"padding": {"top": 20, "right": 30, "bottom": 20, "left": 20}},
        "plugins": {
          "title": {"display": true, "text": "Q4 Financial Summary", "align": "start", "font": {"size": 16, "weight": "600"}, "color": "#18181B", "padding": {"bottom": 20}},
          "legend": {"display": false}
        },
        "scales": {
          "y": {"beginAtZero": true, "border": {"display": false}, "grid": {"color": "#F4F4F5"}, "ticks": {"color": "#71717A", "padding": 10}},
          "x": {"border": {"display": false}, "grid": {"display": false}, "ticks": {"color": "#71717A", "padding": 10}}
        }
      }
    }
  }' \
  --output ~/Downloads/chart_$(date +%Y%m%d_%H%M%S).png
```

### Area Chart
```
/chart area chart: Jan 186, Feb 305, Mar 237, Apr 73, May 209, Jun 214 title "Monthly Visitors"
```

### Multi-Series Comparison
```
/chart line chart with Desktop: Jan 100, Feb 150, Mar 120 and Mobile: Jan 80, Feb 120, Mar 140 title "Traffic by Device"
```

## Output

Charts are saved to `~/Downloads/chart_YYYYMMDD_HHMMSS.png` by default (with timestamp). Use the Read tool to display the generated image to the user.

## No Dependencies

This skill uses QuickChart.io's free API - no local packages required. Just curl.
