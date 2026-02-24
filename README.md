# /diagram

A Claude Code command that reads your code and generates Mermaid diagrams. Built for visualizing scripts, automations, and pipelines.

![Claude Code](https://img.shields.io/badge/Claude_Code-command-blueviolet)
![Mermaid](https://img.shields.io/badge/output-Mermaid-ff69b4)
![Zero Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen)

## Demo

```
> /diagram
```

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
git clone https://github.com/liamc225/diagram-skill.git
cp diagram-skill/diagram.md ~/.claude/commands/diagram.md

# Or per-project
mkdir -p .claude/commands
cp diagram-skill/diagram.md .claude/commands/diagram.md
```

## Usage

```bash
/diagram                        # auto-detect best diagram type
/diagram flow                   # flowchart with decision points
/diagram sequence               # request lifecycle
/diagram architecture           # system overview
/diagram flow src/pipeline      # scope to a directory
```

Saves `DIAGRAM.md` to the project root and opens a live preview in [mermaid.live](https://mermaid.live).

## Examples

See [examples/](./examples/) for more generated diagrams.

## License

MIT
