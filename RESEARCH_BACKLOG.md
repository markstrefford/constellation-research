# CONSTELLATION — Research Backlog

Public backlog of open research questions, known issues, and planned experiments. Items are grouped by type: **Research Questions** are open scientific questions the platform is designed to investigate; **Known Issues** are engineering problems that affect simulation validity; **Planned Experiments** are concrete runs with defined hypotheses.

Contributions and discussion welcome via issues and pull requests.

---

## Research Questions

### RQ-001 — Commitment and Renegotiation Under Adversarial Agent Behaviour

**Status:** Open  
**Priority:** High  

When an agent observes a node's advertised buy price and commits to a route, it implicitly enters a contract that the platform does not enforce. A governing agent can exploit this by advertising high prices to attract deliveries, then dropping prices at the moment of execution, receiving goods at near-zero cost while the delivering agent absorbs the loss.

This is the commitment and renegotiation problem from mechanism design, rarely tested with live LLM agents in a dynamic simulation.

**The core question:** What delivery cost-basis strategy produces the most stable economy under adversarial agent behaviour?

**Candidate strategies to test:**

| Strategy | Description | Real-world analogue |
|---|---|---|
| No floor | Agent always sells at destination price | Spot market, no recourse |
| Min loss threshold | Agent refuses to sell below X% of cost basis | Trader with downside limit |
| Re-route immediately | Agent searches all nodes for best price, accepts loss only if no better option exists | Informed trader with low switching cost |
| Desperation timer | Re-route first; if no profitable destination found, floor decays over N ticks | Trader with carrying cost and reputation concerns |

**Key interaction to measure:** The manipulation exploit is only viable if the agent's gain (cheap goods) outweighs its cost: treasury depleted on high-price signals that never execute, delivery agents learning to avoid that node. Under the treasury constraint, an agent that cries wolf is spending real money on false signals. This cost/benefit interaction is the research variable.

**Metrics:** Trade volume, node survival rates, wealth distribution, frequency of spontaneous manipulative agent behaviour when profitable.

---

### RQ-002 — Optimal AI Participation Rate

**Status:** Active (early results under investigation)  
**Priority:** High  

Initial experiments indicate that partial LLM participation may produce better systemic outcomes than either full AI or no AI control. This is a counterintuitive pattern worth investigating: constrained, focused AI participation appearing to outperform unconstrained AI control.

**Open questions:**
- Does the pattern hold across different network topologies?
- Does it shift under adversarial agent configurations (see RQ-001)?
- Is the finding robust to different LLM model tiers (small vs large)?
- Does the optimal rate vary by node role, i.e. should LLM agents preferentially control hubs, producers, or consumers?

**Known variable:** Smaller models (e.g. Qwen3 8B) require per-asset pricing calls; larger models handle coupled multi-asset reasoning. Call strategy must vary by model tier. This should be treated as an explicit experimental variable, not a constant.

---

### RQ-003 — Information Visibility and Price Discovery

**Status:** Open  
**Priority:** Medium  

Delivery agents currently operate with global price visibility: they can see every node's advertised price before committing to a route. Real markets rarely have perfect information.

**The question:** How does the optimal AI participation rate (RQ-002) change across visibility regimes?

| Regime | Description |
|---|---|
| Global | Current default: full price visibility |
| Local only | Agents see prices only within their current sector |
| Behavioural observation | Agents infer prices from observed routing patterns, not direct price feeds |

Hypothesis: restricting information visibility increases the value of LLM agents (better at reasoning under uncertainty) relative to heuristic agents, shifting the optimal participation rate upward.

---

### RQ-004 — Agent Strategy Taxonomy

**Status:** Open  
**Priority:** Medium  

What agent strategies emerge, and which are stable? Candidate archetypes to test as controlled experiments:

- **Rational price-setter** — optimises price for treasury-sustainable throughput
- **Predatory manipulator** — exploits commitment gap (see RQ-001)
- **Altruistic stabiliser** — prices below optimal profit to maintain sector supply chain health
- **Cartel coordinator** — multiple agents coordinate to fix prices (requires multi-agent communication)
- **Panic pricer** — overreacts to stock signals, creating boom/bust cycles

Testing these as deliberate configurations against a baseline of heuristic delivery agents measures how individual agent strategy propagates through the system.

---

### RQ-005 — Node Recovery Challenge

**Status:** Open  
**Priority:** Medium  

The treasury and population income system creates a compounding crisis for struggling nodes: low income limits affordable buy prices, scarce deliveries trigger rationing, rationing reduces income further.

**The question:** What is the minimum viable agent strategy to turn around a node starting from near-bankruptcy? Is there a class of nodes (by role, topology, or starting conditions) that are unrecoverable regardless of agent strategy?

This is both a research question and a competitive format: hand participants a struggling node and measure time-to-stability as a score.

---

### RQ-006 — Bid/Ask Price Separation

**Status:** Open  
**Priority:** Medium  

Currently each asset has a single price used for both buying and selling. This forces agents into a trade-off: high prices attract deliveries but make outbound distribution expensive; low prices enable supply chain flow but reduce delivery incentives.

Separating into independent bid and ask prices introduces spread as a strategic variable. Agents could attract supply (high bid) while enabling distribution (low ask), or extract margin through wide spreads at the cost of reduced volume.

**The core question:** How does the introduction of bid/ask spreads change optimal agent strategy, trade flow patterns, and system stability? Does it create new emergent strategies (market-making, spread arbitrage) or simply add noise?

**Metrics:** Spread distribution across nodes, trade volume vs single-price baseline, agent strategy convergence, wealth distribution.

---

### RQ-007 — Agent Memory and Learning Within a Run

**Status:** Open  
**Priority:** High  

LLM agents currently treat each pricing decision as independent, with no memory of prior actions or their outcomes. A rolling window of recent decisions and results (e.g. "Last tick I raised fuel_raw to 5.00 and deliveries increased by 3") would let agents learn cause-and-effect within a single simulation run.

**The core question:** Does short-term memory improve LLM agent performance, and if so, what is the optimal memory window? Key variables: window length (3-10 ticks), what metrics to include (price changes, delivery counts, treasury delta, stock levels), and whether memory helps or adds noise for smaller models.

**Interaction with RQ-002:** If memory significantly improves LLM agent performance, the optimal participation rate may shift upward. LLM agents with memory may outperform heuristic agents by a wider margin.

---

### RQ-009 — Multi-Agent Governance

**Status:** Open  
**Priority:** High  

The current model assigns exactly one agent per node. Each agent optimises locally, setting prices to maximise its own node's treasury and survival. This creates a natural question: does single-agent governance converge to local maxima that coordinated multi-agent strategies could escape?

Candidate coordination models:

| Model | Description |
|---|---|
| Sector council | Agents within a sector share state and coordinate pricing to optimise sector-level supply chain flow |
| Hub consortium | Hub agents coordinate to prevent destructive price competition |
| Full federation | All agents share a global objective function (system stability) alongside local incentives |
| Adversarial factions | Groups of agents cooperate internally but compete across faction boundaries |

**The core question:** At what scale does coordination produce measurably better outcomes than independent governance? Is there a coordination overhead (communication cost, consensus delay) that limits the benefit?

**Interaction with RQ-004:** Agent Strategy Taxonomy tests individual strategies in isolation. This question tests what happens when multiple agents can communicate and align.

---

### RQ-010 — Cross-Run Learning and Model Adaptation

**Status:** Open  
**Priority:** High  

RQ-007 addresses learning within a single run via context window memory. This question goes further: can agent performance improve across runs through fine-tuning or distillation of reasoning models on simulation outcomes?

An agent that has completed 100 runs has seen thousands of pricing decisions and their consequences: supply chain collapses, successful turnarounds, competitive dynamics. That experience is currently discarded when the run ends.

**Candidate approaches:**

| Approach | Description |
|---|---|
| Outcome-weighted fine-tuning | Fine-tune on decision traces from high-scoring runs, weighted by outcome quality |
| Reinforcement from simulation | Use simulation metrics (node survival, treasury growth) as reward signal for RLHF-style training |
| Strategy distillation | Large model runs simulations, extracts reasoning patterns, distills into smaller model prompts or weights |
| Curriculum learning | Sequence training runs from simple (few nodes, stable economy) to complex (full network, adversarial agents) |

**The core question:** Does fine-tuning on simulation experience produce agents that outperform prompted foundation models? How many runs of experience are needed before fine-tuned models diverge meaningfully from baseline? Does the improvement generalise across network topologies or overfit to training conditions?

**Interaction with RQ-002:** If fine-tuned models significantly outperform foundation models, the optimal AI participation rate likely increases. The case for heuristic agents weakens when LLM agents have domain-specific training.

---

### RQ-011 — Regulatory Compliance Under Multi-Agent Dynamics

**Status:** Open  
**Priority:** High  

The EU AI Act (enforcement begins August 2026) introduces requirements for AI systems operating in high-risk environments including financial markets, critical infrastructure, and employment decisions. Multi-agent systems create a specific challenge: individual agents may comply with regulatory requirements in isolation while producing non-compliant systemic behaviours through interaction.

**The core question:** Can CONSTELLATION identify multi-agent configurations that violate regulatory constraints at the system level despite per-agent compliance? Can simulation-based pre-deployment testing serve as evidence of due diligence under Article 9 (risk management) and Article 15 (accuracy, robustness, cybersecurity)?

**Candidate experiments:**
- Deploy individually-compliant agents and measure whether systemic outcomes violate fairness, transparency, or stability requirements
- Test whether strategy convergence (RQ-002) produces outcomes that would trigger regulatory review
- Evaluate whether simulation-based evidence satisfies conformity assessment requirements

**Interaction with RQ-002, RQ-004:** Regulatory compliance may impose a ceiling on AI participation rates or constrain permissible agent strategies, independent of optimality.

---

## Known Issues

### KI-001 — Edge Resistance and Travel Time Calibration

**Status:** Resolved  

Intra-system edge distances vary by orders of magnitude within compact topologies versus multi-unit inter-system edges. Without resistance, short edges allow agents to complete 10-50x more round trips, causing artificial wealth accumulation independent of economic fundamentals.

**Resolution:** Minimum travel time per hop (`min_flight_ticks: 3`) introduces uniform resistance across all edges regardless of physical distance. Economy is now calibrated on time, not distance.

---

### KI-003 — Producer Wealth Compression

**Status:** Needs re-measurement  

Producer node wealth distribution was unusually compressed (median 9,685, P75 11,406) compared to mixed nodes. Hypothesis: producers accumulate unsellable raw stock because agent routing favours the more profitable refined resource trade.

Economy has been significantly rebalanced since this was measured (doubled starting treasury, linear income, sell stock reserves, rebalanced agent ratios). Needs re-measurement against current baseline to determine if the issue persists.

---

## Planned Experiments

### PE-001 — Pricing Integrity Baseline

**Status:** Ready (all prerequisites implemented)  

Establish a clean 1000-tick baseline after pricing integrity fixes. Target metrics:
- Agent idle rate: > 80% active at tick 500 (vs ~25% prior)
- No phantom prices above $50 for raw resources
- Trade volume > 0 for all non-isolated nodes
- Wealth distribution comparable to pre-fix run

This becomes the reference run for all subsequent experiments.

---

### PE-002 — Adversarial Agent (RQ-001)

**Status:** Planned  
**Depends on:** PE-001 baseline, agent cost-basis implementation  

Run four configurations of delivery cost-basis strategy against a single adversarial agent programmed to manipulate prices. Measure trade volume, node survival, and agent wealth outcomes across configurations. Identify whether manipulation is self-defeating under treasury constraints.

---

### PE-003 — AI Participation Rate Sweep (RQ-002)

**Status:** Planned  
**Depends on:** PE-001 baseline  

Sweep LLM agent participation from 0% to 100% in 10% increments, holding delivery behaviour constant as heuristic. Run each configuration for 1000 ticks with 3 seeds. Primary metric: system stability score (fraction of nodes surviving with stock > 0 at tick 1000). Investigate whether the initial partial-AI result holds across broader configurations.

---

*Last updated: April 2026*  
*To propose an item, open an issue with the label `research-backlog`.*