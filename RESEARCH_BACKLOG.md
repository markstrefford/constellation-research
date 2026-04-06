# Constellation — Research Backlog

Public backlog of open research questions, known issues, and planned experiments. Items are grouped by type: **Research Questions** are open scientific questions the platform is designed to investigate; **Known Issues** are engineering problems that affect simulation validity; **Planned Experiments** are concrete runs with defined hypotheses.

Contributions and discussion welcome via issues and pull requests.

---

## Research Questions

### RQ-001 — Commitment and Renegotiation Under Adversarial Governor Behaviour

**Status:** Open  
**Priority:** High  

When a courier observes a planet's advertised buy price and commits to a route, it implicitly enters a contract that the platform does not enforce. A Governor bot can exploit this by advertising high prices to attract couriers, then dropping prices at the moment of execution — receiving cargo at near-zero cost while the courier absorbs the loss.

This is the commitment and renegotiation problem from mechanism design, rarely tested with live LLM agents in a dynamic simulation.

**The core question:** What courier cost-basis strategy produces the most stable economy under adversarial Governor behaviour?

**Candidate strategies to test:**

| Strategy | Description | Real-world analogue |
|---|---|---|
| No floor | Courier always sells at destination price | Spot market, no recourse |
| Min loss threshold | Courier refuses to sell below X% of cost basis | Trader with downside limit |
| Re-route immediately | Courier searches all planets for best price, accepts loss only if no better option exists | Informed trader with low switching cost |
| Desperation timer | Re-route first; if no profitable destination found, floor decays over N ticks | Trader with carrying cost and reputation concerns |

**Key interaction to measure:** The manipulation exploit is only viable if the Governor's gain (cheap cargo) outweighs its cost — treasury depleted on high-price signals that never execute, couriers learning to avoid that planet. Under the treasury constraint, a Governor that cries wolf is spending real money on false signals. This cost/benefit interaction is the research variable.

**Metrics:** Trade volume, planet survival rates, wealth distribution, frequency of spontaneous manipulative Governor behaviour when profitable.

---

### RQ-002 — Optimal LLM Participation Rate

**Status:** Active (prior finding: ~25–27%)  
**Priority:** High  

Prior simulation results indicate that approximately 25–27% LLM Governor participation produces optimal system stability, outperforming both fully heuristic and fully LLM-driven configurations. This is a counterintuitive result — constrained, focused AI participation outperforms unconstrained AI control.

**Open questions:**
- Does the optimal rate hold across different galaxy topologies?
- Does it shift under adversarial Governor configurations (see RQ-001)?
- Is the finding robust to different LLM model tiers (small vs large)?
- Does optimal rate vary by planet role — i.e., should LLM Governors preferentially control hubs, producers, or consumers?

**Known variable:** Smaller models (e.g. Qwen3 8B) require per-asset pricing calls; larger models handle coupled multi-asset reasoning. Call strategy must vary by model tier — this should be treated as an explicit experimental variable, not a constant.

---

### RQ-003 — Information Visibility and Price Discovery

**Status:** Open  
**Priority:** Medium  

Couriers currently operate with global price visibility — they can see every planet's advertised price before committing to a route. Real markets rarely have perfect information. 

**The question:** How does optimal LLM Governor participation rate (RQ-002) change across visibility regimes?

| Regime | Description |
|---|---|
| Global | Current default — full price visibility |
| Local only | Couriers see prices only within their current sector |
| Behavioural observation | Couriers infer prices from observed courier routing patterns, not direct price feeds |

Hypothesis: restricting information visibility increases the value of LLM Governors (better at reasoning under uncertainty) relative to heuristic agents, shifting the optimal participation rate upward.

---

### RQ-004 — Governor Strategy Taxonomy

**Status:** Open  
**Priority:** Medium  

What Governor strategies emerge, and which are stable? Candidate archetypes to test as controlled experiments:

- **Rational price-setter** — optimises price for treasury-sustainable throughput
- **Predatory manipulator** — exploits commitment gap (see RQ-001)
- **Altruistic stabiliser** — prices below optimal profit to maintain sector supply chain health
- **Cartel coordinator** — multiple Governors coordinate to fix prices (requires multi-agent communication)
- **Panic pricer** — overreacts to stock signals, creating boom/bust cycles

Testing these as deliberate configurations against a baseline of heuristic couriers measures how individual Governor strategy propagates through the system.

---

### RQ-005 — Planet Turnaround Challenge

**Status:** Open  
**Priority:** Medium  

The treasury and population income system creates a compounding crisis for struggling planets: low income limits affordable buy prices, scarce deliveries trigger rationing, rationing reduces income further.

**The question:** What is the minimum viable Governor strategy to turn around a planet starting from near-bankruptcy? Is there a class of planets (by role, topology, or starting conditions) that are unrecoverable regardless of Governor strategy?

This is both a research question and a competitive format — hand participants a struggling planet and measure time-to-stability as a score.

---

## Known Issues

### KI-001 — TRAPPIST-1 Distance Distortion

**Status:** Open — fix under discussion  

TRAPPIST-1 intra-system lane distances (0.004–0.059 ly) are astronomically accurate but economically distorting. Couriers complete 10–50× more round trips per 1000 ticks in that system than in any other sector, causing artificial wealth accumulation: trappist1-c (refinery, wealth 32K), trappist1-g (mixed, 29K), and trappist1-h (mixed, 28K) are the 2nd, 3rd, and 4th wealthiest non-hub planets in the galaxy despite no structural economic advantage.

**Proposed fix:** Minimum travel time per hop regardless of physical distance. Economy should be calibrated on time, not light-years.

---

### KI-002 — Sol-Mercury Zero Trades

**Status:** Under investigation  

Sol-Mercury (consumer, fuel_refined only) recorded zero buy and zero sell transactions across a 1000-tick run. Probable cause: Sol-hub produces fuel_refined directly, keeping hub prices low enough that Mercury's price never spikes high enough to attract a dedicated courier route — Mercury is being passively supplied or slowly dying without ever generating a trade signal.

Needs stock-level trace to confirm whether Mercury is surviving on ambient supply or depleting silently.

---

### KI-003 — Producer Wealth Compression

**Status:** Open  

Producer planet wealth distribution is unusually compressed (median 9,685, P75 11,406) compared to mixed planets (median 2,900, P75 5,080 but outliers to 29K). Hypothesis: producers are accumulating unsellable raw fuel stock (confirmed: trappist1-b holding 25K fuel_raw) because courier routing favours the more profitable refined fuel trade. 

Producers have healthy population income but low trade revenue. Needs courier routing analysis to confirm.

---

### KI-004 — Duplicate Lanes in Galaxy Config

**Status:** Open — low priority  

Every lane in `stellar_neighborhood.yaml` appears twice. Does not break routing but inflates traversal computations and may affect congestion calculations. Clean-up deferred until after pricing fixes stabilise.

---

### KI-005 — Cargo Type Distribution

**Status:** Open — deferred  

Approximately 75% of couriers currently carry food. This may be too high, concentrating trade volume in the food supply chain and underserving the fuel chain. Deferred until pricing fixes (treasury cap, partial fills, derived demand removal) stabilise the baseline.

---

## Planned Experiments

### PE-001 — Pricing Integrity Baseline

**Status:** Planned — pending implementation  
**Depends on:** Treasury cap + partial fills + derived demand removal

Establish a clean 1000-tick baseline after pricing integrity fixes. Target metrics:
- Agent idle rate > 80% active at tick 500 (vs ~25% current)
- No phantom prices above $50 for raw fuel
- Trade volume > 0 for all non-isolated planets
- Wealth distribution comparable to pre-fix run

This becomes the reference run for all subsequent experiments.

---

### PE-002 — Adversarial Governor (RQ-001)

**Status:** Planned  
**Depends on:** PE-001 baseline, courier cost-basis implementation  

Run four configurations of courier cost-basis strategy against a single adversarial Governor bot programmed to manipulate prices. Measure trade volume, planet survival, and courier wealth outcomes across configurations. Identify whether manipulation is self-defeating under treasury constraints.

---

### PE-003 — LLM Participation Rate Sweep (RQ-002)

**Status:** Planned  
**Depends on:** PE-001 baseline  

Sweep LLM Governor participation from 0% to 100% in 10% increments, holding courier behaviour constant as heuristic. Run each configuration for 1000 ticks with 3 seeds. Primary metric: system stability score (fraction of planets surviving with stock > 0 at tick 1000). Replicate the 25–27% finding on the current galaxy topology.

---

*Last updated: March 2026*  
*To propose an item, open an issue with the label `research-backlog`.*
