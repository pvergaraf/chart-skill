# Chart Skill for Claude Code

Generate beautiful Economist-style charts using QuickChart.io. No dependencies required - just curl.

## Installation

```bash
npx skills add pvergaraf/chart-skill
```

## Features

- Bar, line, pie, scatter, area, and horizontal bar charts
- Economist-inspired color palette and styling
- Data labels, clean gridlines, left-aligned titles
- Outputs to `~/Downloads/` with automatic timestamps
- No dependencies - uses QuickChart.io API

## Usage

```
/chart bar chart: Revenue 450, Costs 320, Profit 130 title "Q4 Summary"
/chart line chart: Jan 100, Feb 150, Mar 200, Apr 180
/chart pie chart: Sales 45%, Marketing 30%, Engineering 25%
/chart horizontal bar: Chile 89, Mexico 76, Peru 65
```

## License

MIT
