# /diagram

A Claude Code command that reads your code and generates Mermaid diagrams. Built for visualizing scripts, automations, and pipelines.

![Claude Code](https://img.shields.io/badge/Claude_Code-command-blueviolet)
![Mermaid](https://img.shields.io/badge/output-Mermaid-ff69b4)
![Zero Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen)

## Demo

Running `/diagram` on a Python flight deal tracker I built:

```mermaid
graph TD
    Cron(("macOS launchd<br/>daily @ 8am"))

    subgraph FlightTracker["flight-tracker"]
        Checker["flight_checker.py<br/>Orchestrator"]
        DealEngine["Deal Detection<br/>threshold + rolling median"]
        History[("price_history.json")]
        Notifier["notifier.py<br/>HTML email builder"]
    end

    subgraph APIs["Flight Data APIs"]
        Amadeus["Amadeus API<br/>primary"]
        RapidAPI["RapidAPI<br/>Google Flights<br/>fallback"]
    end

    SMTP["Gmail SMTP"]
    User((User))

    Cron --> Checker
    Checker -->|OAuth2 + search| Amadeus
    Checker -.->|"fallback if empty"| RapidAPI
    Checker --> DealEngine
    DealEngine <-->|read/write| History
    DealEngine -->|deals found| Notifier
    Notifier -->|SMTP/TLS| SMTP
    SMTP -->|email alert| User

    style FlightTracker fill:#1a1a2e,stroke:#6366f1,color:#e2e8f0
    style APIs fill:#1a1a2e,stroke:#f59e0b,color:#e2e8f0
    style Cron fill:#22d3ee,stroke:#22d3ee,color:#0f172a
    style User fill:#6366f1,stroke:#6366f1,color:#fff
    style SMTP fill:#0f172a,stroke:#ef4444,color:#ef4444
    style History fill:#0f172a,stroke:#22d3ee,color:#22d3ee
```

## Install

```bash
# Global (works in every project)
curl -o ~/.claude/commands/diagram.md \
  https://raw.githubusercontent.com/liamc225/diagram-skill/main/diagram.md

# Or clone the repo
git clone https://github.com/liamc225/diagram-skill.git
cp diagram-skill/diagram.md ~/.claude/commands/diagram.md
```

## How it works

1. `cd` into any project and run `/diagram`
2. Claude reads your source files — entry points, configs, imports, API calls
3. It picks the best diagram type for what it finds (or you can specify one)
4. Outputs a color-coded Mermaid diagram capped at 8-12 nodes
5. Saves `DIAGRAM.md` to the project root
6. Opens a live preview in [mermaid.live](https://mermaid.live) where you can edit and export PNG/SVG

The command is a single markdown file — no dependencies, no scripts, no API keys. It works by giving Claude structured instructions for how to analyze code and produce diagrams.

## Usage

```bash
/diagram                        # auto-detect best diagram type
/diagram architecture           # system overview with external services
/diagram flow                   # flowchart with decision points
/diagram sequence               # request lifecycle (API calls, webhooks)
/diagram erd                    # database schema from models
/diagram architecture src/api   # scope to a specific directory
```

Works best on:
- Python scripts and automation pipelines
- Cron jobs and scheduled tasks
- API integrations with external services
- Next.js / Express apps with clear route structures

## Examples

See [examples/](./examples/) for diagrams generated from real projects.

## License

MIT
