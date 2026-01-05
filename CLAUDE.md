# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file educational web application: an **AEX Management Simulator** for business students. It demonstrates strategic investment decision-making using Monte Carlo simulations based on historical AEX (Amsterdam Exchange Index) data from 1984-2023.

## Architecture

**Single-File Application Structure** ([index.html](index.html)):
- Pure client-side HTML with inline React (via Babel standalone)
- No build process, no npm dependencies
- CDN-loaded libraries: React 18.2.0, Recharts 2.12.7, Tailwind CSS
- All JavaScript is in a `<script type="text/babel">` block

**Core Components:**
- `App` component manages all state and simulation logic
- Two main tabs: "Simulator" and "Uitleg" (Explanation)
- Simulation engine uses historical bootstrapping (sampling with replacement from historical returns)

**Key State Variables:**
- `riskFreeRate`: Annual risk-free return percentage (default: 3%)
- `beta`: Portfolio beta coefficient for volatility adjustment (default: 1.0)
- `numSimulations`: Monte Carlo iterations (100/1000/5000)
- `results`: Computed simulation outcomes (chart data, statistics, percentiles)

**Simulation Algorithm:**
1. Samples random years from historical AEX returns (40 years of data)
2. Adjusts returns by beta coefficient
3. Runs N simulations over 10-year horizon
4. Computes percentiles (P10, median, P90) and risk metrics

## Development Workflow

**Running the Application:**
```bash
# Simply open index.html in a browser
open index.html  # macOS
# Or use a local server:
python3 -m http.server 8000  # Then visit http://localhost:8000
```

**No Build System:**
- Changes to index.html are immediately reflected on browser refresh
- No compilation, bundling, or transpilation required (Babel runs in-browser)

## Data Structure

**Historical Returns:**
- Embedded CSV data in `rawData` string (lines 122-165)
- Parsed into `historicalReturns` array via useMemo
- 40 annual return values from 1984-2023

**Chart Data Format:**
Each data point contains:
- `year`: Label (e.g., "Jaar 0", "Jaar 1")
- `riskFree`: Guaranteed savings outcome
- `median`: Median investment outcome
- `p10`, `p90`: 10th and 90th percentile bounds
- `sim0` through `sim7`: Individual simulation paths for visualization

## UI Components

**Recharts Visualization:**
- `ComposedChart` combines Area (confidence band), Line (trajectories), and statistics
- Orange dashed line = risk-free savings path
- Blue solid line = median investment path
- Blue shaded area = 80% confidence interval (P10 to P90)
- 8 faint sample paths for visual texture

**Interactive Controls:**
- Range sliders for riskFreeRate and beta
- Dropdown selector for numSimulations
- Results auto-calculate on mount; manual refresh via button

## Language & Audience

- **Interface language:** Dutch (nl)
- **Target audience:** Business students (bedrijfskunde studenten)
- Labels use Dutch terminology:
  - "Sparen" = Savings
  - "Beleggen" = Investing
  - "Risicovrije Voet" = Risk-free rate
  - "Risico-Index" = Risk index

## Key Metrics Displayed

- **Risico-Index:** Probability that investment underperforms savings (%)
- **"Geluksvogel" Scenario:** Best-case outcome from all simulations
- **"Pechvogel" Scenario:** Worst-case outcome from all simulations
- Final values at year 10 for both strategies
