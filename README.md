# Chart Skill

Generate clean, minimal charts with shadcn-inspired grayscale styling. No dependencies required - just curl.

Works with Claude Code, Cursor, Cline, and other AI agents that support the Agent Skills standard.

## Installation

```bash
npx skills add pvergaraf/chart-skill
```

## Features

- Bar, line, area, pie, doughnut, and horizontal bar charts
- shadcn/Zinc grayscale color palette
- Minimal design with subtle gridlines and rounded corners
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
