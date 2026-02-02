---
name: chart
description: Generate beautiful Economist-style charts using QuickChart.io. Supports bar, line, pie, doughnut, radar, and scatter charts. No dependencies required.
allowed-tools:
  - Bash(curl *)
  - Read
  - Write
---

# Chart Generation Skill

Generate publication-quality charts in the style of The Economist's Graphic Detail using QuickChart.io (no local dependencies needed).

## Quick Usage

```
/chart bar chart: Category A 45, Category B 32, Category C 78
/chart line chart: Jan 100, Feb 150, Mar 200, Apr 180
/chart pie chart: Sales 45%, Marketing 30%, Engineering 25%
/chart horizontal bar: Product A 120, Product B 85, Product C 200
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
| Pie | `pie`, `pie chart` | Proportions |
| Doughnut | `doughnut`, `donut` | Proportions (ring style) |
| Radar | `radar` | Multi-dimensional comparison |
| Scatter | `scatter` | Correlations |

## Options

- **title**: `title "My Chart Title"`
- **output**: `save to /path/to/chart.png` (default: `~/Downloads/chart_YYYYMMDD_HHMMSS.png` with timestamp)
- **size**: `size 800x600` (width x height in pixels, default: 600x400)

## Color Palette (Economist-inspired)

| Color | Hex | Usage |
|-------|-----|-------|
| Primary Red | `#E3120B` | Main data series |
| Blue | `#0D6ABF` | Secondary series |
| Teal | `#006D64` | Third series |
| Orange | `#F18F01` | Fourth series |
| Gray | `#4A4A4A` | Fifth series |
| Dark Red | `#9E2A2B` | Sixth series |

## Economist Style Elements

- Clean, minimal design
- Sans-serif fonts
- Horizontal gridlines only
- No chart border on top and right
- Data labels on charts
- Left-aligned titles

## Instructions

When the user requests a chart:

1. **Parse the request** to identify:
   - Chart type (bar, horizontalBar, line, pie, doughnut, radar, scatter)
   - Data (inline or file path)
   - Title (if provided)
   - Output path (default: `~/Downloads/chart_$(date +%Y%m%d_%H%M%S).png`)
   - Size (default: 600x400)

2. **Build the Chart.js configuration** using this Economist-style template:

```json
{
  "type": "bar",
  "data": {
    "labels": ["A", "B", "C"],
    "datasets": [{
      "label": "Data",
      "data": [10, 20, 30],
      "backgroundColor": ["#E3120B", "#0D6ABF", "#006D64", "#F18F01", "#4A4A4A", "#9E2A2B"]
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
        "font": { "size": 18, "weight": "bold", "family": "Helvetica Neue, Arial, sans-serif" },
        "color": "#1A1A1A",
        "padding": { "bottom": 20 }
      },
      "legend": { "display": false },
      "datalabels": {
        "display": true,
        "anchor": "end",
        "align": "top",
        "color": "#1A1A1A",
        "font": { "weight": "bold" }
      }
    },
    "scales": {
      "y": {
        "beginAtZero": true,
        "grid": { "color": "#D9D5C9" },
        "ticks": { "color": "#1A1A1A", "padding": 10 }
      },
      "x": {
        "grid": { "display": false },
        "ticks": { "color": "#1A1A1A", "padding": 10 }
      }
    }
  }
}
```

3. **Generate the chart** using curl to QuickChart.io:

```bash
curl -o ~/Downloads/chart_$(date +%Y%m%d_%H%M%S).png "https://quickchart.io/chart?c=$(echo 'CHART_CONFIG_JSON' | jq -sRr @uri)&backgroundColor=white&width=600&height=400"
```

Use POST with `"version": "4"` for Chart.js v4 syntax:

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

### Bar Chart (vertical)
```json
{
  "type": "bar",
  "data": {
    "labels": ["A", "B", "C"],
    "datasets": [{"data": [10, 20, 30], "backgroundColor": "#E3120B"}]
  }
}
```

### Horizontal Bar Chart
```json
{
  "type": "bar",
  "data": {
    "labels": ["A", "B", "C"],
    "datasets": [{"data": [10, 20, 30], "backgroundColor": "#E3120B"}]
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
      "borderColor": "#E3120B",
      "backgroundColor": "rgba(227, 18, 11, 0.1)",
      "fill": true,
      "tension": 0.1
    }]
  }
}
```

### Pie Chart
```json
{
  "type": "pie",
  "data": {
    "labels": ["A", "B", "C"],
    "datasets": [{
      "data": [45, 30, 25],
      "backgroundColor": ["#E3120B", "#0D6ABF", "#006D64"]
    }]
  }
}
```

### Multi-Series Line Chart
```json
{
  "type": "line",
  "data": {
    "labels": ["Jan", "Feb", "Mar"],
    "datasets": [
      {"label": "Series A", "data": [10, 20, 30], "borderColor": "#E3120B"},
      {"label": "Series B", "data": [15, 25, 20], "borderColor": "#0D6ABF"}
    ]
  },
  "options": {
    "plugins": { "legend": { "display": true, "position": "top" } }
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
        "datasets": [{"data": [450, 320, 130], "backgroundColor": ["#E3120B", "#0D6ABF", "#006D64"]}]
      },
      "options": {
        "layout": {"padding": {"top": 20, "right": 30, "bottom": 20, "left": 20}},
        "plugins": {
          "title": {"display": true, "text": "Q4 Financial Summary", "align": "start", "font": {"size": 18, "weight": "bold"}, "padding": {"bottom": 20}},
          "legend": {"display": false},
          "datalabels": {"display": true, "anchor": "end", "align": "top", "font": {"weight": "bold"}}
        },
        "scales": {
          "y": {"beginAtZero": true, "grid": {"color": "#D9D5C9"}, "ticks": {"padding": 10}},
          "x": {"grid": {"display": false}, "ticks": {"padding": 10}}
        }
      }
    }
  }' \
  --output ~/Downloads/chart_$(date +%Y%m%d_%H%M%S).png
```

### Horizontal Bar Chart (Rankings)
```
/chart horizontal bar: Chile 89, Mexico 76, Peru 65, Colombia 58 title "Market Share by Country"
```

## Output

Charts are saved to `~/Downloads/chart_YYYYMMDD_HHMMSS.png` by default (with automatic timestamp to avoid overwriting). Use the Read tool to display the generated image to the user.

## No Dependencies

This skill uses QuickChart.io's free API - no local Python or npm packages required. Just curl.
