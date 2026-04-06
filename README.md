# CONSTELLATION — Multi-Agent Development and Evaluation Platform

CONSTELLATION is a simulation engine for developing, evaluating, and stress-testing autonomous AI agents in controlled economic environments. It surfaces systemic behaviours that only emerge when agents interact at scale: strategy convergence, failure propagation, hidden dependencies.

Supply chains. Financial markets. Operating models.

**Website:** [reimagined.industries/constellation](https://reimagined.industries/constellation)  
**Live simulation:** [app.constellationproject.ai](https://app.constellationproject.ai)  
**Built by:** [Reimagined Industries](https://reimagined.industries)

---

## Early Research Findings

Initial experiments compared different ratios of LLM to heuristic control over economic decision-making in a multi-agent simulation. Early results suggest a counterintuitive pattern.

| Configuration | System Output | Nodes in Crisis |
|---|---|---|
| 0% AI control | $249K | 53% |
| 27% AI control | $338K | **33%** |
| 100% AI control | $352K | 47% |

**Early indication:** In these initial runs, partial AI control produced better systemic outcomes than either full AI or no AI. Higher output than pure heuristics, fewer system failures than full AI. When every node optimised locally, supply chains showed signs of strategy convergence.

These are preliminary findings from a small number of configurations. The pattern is interesting enough to investigate further across broader architecture mixes, population sizes, and scenario types. The research continues.

📁 Full results: [`/research/v1a-llm-pricing/`](research/v1a-llm-pricing/)

### Local Model Benchmarks (February 2026)

Benchmarked local LLMs for real-time hub pricing on Apple Silicon (M2, 16GB). qwen2.5:14b was the clear winner at 2.5s median latency — 18x faster than qwen3:8b, with more decisive pricing behaviour. Thinking-mode models (qwen3) added massive overhead with no quality improvement on structured JSON tasks.

📁 Full results: [`/research/v2-local-models/`](research/v2-local-models/)

---

## Architecture

CONSTELLATION is built as a modular, multi-agent simulation with clean separation of concerns.

### Core Components

- **Agents** — Autonomous traders with fuel, wealth, cargo capacity, and pluggable decision-making. Each agent perceives its environment and chooses actions independently.
- **Nodes** — Economic entities that produce, consume, refine, and store resources. Prices emerge from supply and demand dynamics.
- **Environments** — Connected graphs of nodes with distance, capacity, and congestion mechanics. Configurable topology for different use cases.
- **Events** — Every action logged: trades, movement, price changes, failures. The complete system history is observable and replayable.

### Technical Stack

- **Simulation:** Python / SimPy for discrete-event simulation
- **Visualisation:** React / TypeScript
- **Decision layer:** Pluggable architecture supporting heuristics, local LLMs, or API-based models
- **Persistence:** JSON event logs, YAML configuration, structured run archives
- **Compute:** Modal Labs for scalable simulation runs

### Design Principles

- **Architecture-agnostic** — Evaluate any agent type: heuristic, LLM, hybrid, human-in-the-loop
- **Reproducible** — Same seed produces identical runs
- **Observable** — Every tick, every trade, every decision captured and structured
- **Pluggable** — Agent decision-makers are swappable without changing execution logic

---

## Capabilities

### Develop
Build multi-agent systems in controlled economic environments with real resource constraints, distance costs, and scarcity dynamics. CONSTELLATION provides the world your agents operate in.

### Evaluate
Surface emergent behaviours before deployment. Strategy convergence, failure propagation, hidden dependencies, and system health metrics across hundreds of agent configurations.

### Train
Every simulation run generates over 500,000 structured events. Interaction data, reproducible runs, and architecture benchmarks, structured and ready for training.

---

## Environments

| Environment | Status | Description |
|---|---|---|
| Supply chains | Live | Routing fragility, capacity bottlenecks, knock-on effects across interconnected logistics networks |
| Financial systems | Live | Strategy convergence, systemic exposure, agent behaviour under competing autonomous objectives |
| Operating models | Coming soon | Resource allocation, decision quality, workforce dynamics under automation |

---

## Experiment History

### v0 — Foundation ✅
Core logistics and emergent economics. 20 agents trading across a 15-node network. Demonstrated natural hub formation, wealth stratification from identical starting conditions, and parameter sensitivity at critical thresholds.

### v1a — LLM Decision-Making ✅
Introduced a pluggable LLM layer for pricing strategy at the node level. Agents remained heuristic-driven, responding to price signals set by a mix of LLM and rule-based nodes. Ran initial experiments varying the ratio of AI to heuristic control across the same environment to explore how different levels of autonomy affect system-wide outcomes. Early results were counterintuitive enough to warrant deeper investigation across broader configurations.

### v1b — Experiential Memory 🔄 In progress
Agent memory of past trades, price observations, and outcomes. Research question: how does learning from experience change emergent system dynamics?

### v2 — Planning & Coordination 📋 Planned
Multi-step reasoning, agent-to-agent observation, optional communication channels. Research question: do agents naturally form partnerships, and can they learn to deceive?

---

## BotArena

[BotArena](https://botarena.gg) is a competitive evaluation layer built on CONSTELLATION. Submit your own autonomous agents into a live economic battle royale. Powered by the same simulation engine, open to external participants via MCP.

---

## Research Questions

- How do autonomous agents coordinate under scarcity?
- What systemic risks emerge from strategy convergence in multi-agent deployments?
- How do different agent architectures (heuristic, LLM, hybrid) compare under identical conditions?
- What is the optimal balance of AI and human control for system stability?
- How do these dynamics translate to real-world supply chains, financial markets, and operating models?

Full research backlog: [RESEARCH_BACKLOG.md](RESEARCH_BACKLOG.md)

---

## References

1. Park, J.S., et al. (2023). *Generative Agents: Interactive Simulacra of Human Behavior.* arXiv:2304.03442.
2. Gao, C., et al. (2023). *S3: Social-network Simulation System with Large Language Model-Empowered Agents.* arXiv:2307.14984.
3. Yu, C., et al. (2024). *A Large-Scale Simulation on Large Language Models for Decision-Making.* arXiv:2412.15291.

---

## License

[View license](LICENSE)

---

© 2026 [Reimagined Industries](https://reimagined.industries)
