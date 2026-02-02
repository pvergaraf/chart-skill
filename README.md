# Chart Skill

Generate clean, minimal charts with shadcn-inspired grayscale styling. No dependencies required.

Works with Claude Code, Cursor, Cline, and other AI agents.

## Installation

```bash
npx skills add pvergaraf/chart-skill
```

## Features

- Bar, line, area, pie, doughnut, and horizontal bar charts
- shadcn/Zinc grayscale color palette
- Minimal design with subtle gridlines and rounded corners
- Outputs to `~/Downloads/` with automatic timestamps

## Usage

Just describe what you want:

```
/chart show me monthly revenue: Jan $12k, Feb $15k, Mar $18k, Apr $14k

/chart compare our Q4 performance - Revenue was 450, Costs 320, Profit 130

/chart visualize the team breakdown: Engineering 45%, Product 25%, Design 20%, Operations 10%

/chart create an area chart of daily active users over the past week

/chart I need a horizontal bar ranking: Chile scored 89, Mexico 76, Peru 65, Colombia 58

/chart plot the correlation between marketing spend and signups from this CSV file
```

You can also feed it data from files, paste raw CSV/JSON, or describe trends in plain English.

## License

MIT
