---
title: Whitepaper v2.0
authors:
  - Adethya Srinivasan
status: published
---


# Adaptive Trust: A Probabilistic Reputation Layer for ERC-8004

## Abstract

In decentralised agentic economies (ERC-8004), static reputation scores are insufficient. They fail to capture the nuance of agent behavior, are vulnerable to Sybil attacks, and cannot predict "exit scams"/ "rug-pull" behaviours. This paper introduces the **Beta-Dirichlet Reputation System**, a novel cryptographic trust engine. By treating every agent as a probabilistic entity and utilising a dynamic "Mixture of Betas" landscape, we move beyond binary trust scores to a dynamic, predictive, and Sybil-resistant reputation protocol.

---

## 1. Introduction: The Crisis of Static Trust

Current reputation systems in Web3 rely on scalar accumulation - simple counters of "stars" or "successful transactions." These systems suffer from three critical failures:

1. **Lack of Nuance:** A new agent with 5 successful transactions often looks identical to a veteran with 5,000.
2. **Rigidity:** They fail to detect sudden changes in behavior, such as a high-performing agent turning malicious (an exit scam).
3. **Sybil Vulnerability:** They cannot effectively distinguish between a genuine new entrant and a bot farm "washing" reputation.

We propose a shift from Accumulative Trust to **Probabilistic Trust**. Instead of asking "Is this agent good?", our system asks "What is the probability that this agent will perform X action in the next transaction, given their history and the behavior of similar agents?"

---

## 2. Core Architecture

### 2.1 The Probabilistic Agent

At the heart of our model, no agent is defined by a single number. Instead, every agent is represented by a **multidimensional probability distribution** (specifically, a Beta Distribution). This allows the system to capture two distinct axes of information simultaneously:

- **Performance:** The ratio of successful outcomes to failures.
- **Confidence:** The "weight" of the evidence.

This distinction is critical. An agent with a 100% success rate on 2 transactions is treated fundamentally differently from an agent with a 99% success rate on 1,000 transactions. The former has high potential but low confidence; the latter has high proven reliability.

### 2.2 The Global Landscape: A Mixture of Experts

Agents do not exist in a vacuum. Our system utilises a **Global Mixture Model** that dynamically categorises agents into behavioral archetypes (clusters).

Rather than applying a universal rule to all participants, the system observes the entire economy and identifies "clusters" of behavior. For example, the system might autonomously identify:

- **"High Performers":** High volume, high success, low variance.
- **"The Gamblers":** High volume, inconsistent results.
- **"The Malicious":** Agents that frequently default or illustrate adversarial behaviours.

Every agent is dynamically assigned to the cluster that best fits their recent history. This allows the system to apply different predictive baselines to different types of agents.

---

## 3. The Trust Engine Mechanism

### 3.1 Interaction & Evidence Weighting

Not all feedback is created equal. A common attack vector in decentralised reputation is "Review Bombing" or "Self-Dealing," where an attacker uses low-value transactions to inflate their score.

Our engine counters this via a **Consumer-Weighted Evidence Mass**. The impact of a review is scaled by:

1. **Transaction Value:** High-stakes interactions carry more weight (logarithmically scaled) than micro-transactions.

2. **Reviewer Credibility:** The system assesses the "Trust Function" of the reviewer. Feedback from a fresh wallet with no history is dampened, while feedback from a high-reputation consumer applies full force.

### 3.2 Temporal Decay (The "What Have You Done Lately?" Protocol)

Reputation should not be permanent. To prevent "Lazy Incumbency" - where an old agent rests on laurels earned years ago - the system applies a continuous **Ornstein-Uhlenbeck Decay**.

Agent history effectively "evaporates" over time if not reinforced. This ensures:

- **Redemption:** Agents who made mistakes can eventually improve their standing by sustaining good behavior.
- **Exit Scam Detection:** If a good agent stops performing, their confidence interval widens, and their score degrades, alerting users to the inactivity.

### 3.3 Dynamic Re-Assignment (The "Drift" Logic)

This is the engine's primary defense mechanism. When an agent's behavior changes (e.g., a "Good" agent starts failing), the mathematical likelihood of them belonging to a "High Trust" cluster drops precipitously.

The system performs a **Bayesian Model Selection** at every step. If an agent's behavior deviates from their current archetype, they are statistically "ejected" and re-assigned to a cluster that better represents their new reality (e.g., moving from "Reliable" to "Volatile"). This re-assignment happens instantly, causing a sharp drop in their public Trust Score before they can inflict widespread damage.

---

## 4. Security & Threat Mitigation

The Beta-Dirichlet architecture is explicitly designed to counter the ERC-8004 threat landscape.

|**Threat Vector**|**The Defense Mechanism**|
|---|---|
|**Whitewashing** (Creating a new ID to wipe bad history)|**The "Nursery" Protocol:** All new agents enter a restricted "New Entrant" cluster (The Nursery). They possess a "sticky" history. They cannot access high-value trust tiers until they satisfy a graduated "Volatility Threshold." Re-setting your ID forces you back to the Nursery.|
|**Exit Scams** (Building trust, then defecting)|**Cluster Ejection:** As soon as a high-performing agent registers failures, the probability density of them belonging to the "Elite" cluster collapses. They are mathematically forced into a "volatile" cluster, tanking their utility score immediately.|
|**Sybil Attacks** (Spamming fake agents)|**The "Cold Start" Prior:** Because confidence is a dimension of the score, 1,000 Sybils with 1 transaction each will have a significantly lower aggregate trust score than 1 agent with 1,000 transactions. The math favors depth over breadth.|
|**Wealth Bias**|**Saturation Caps:** While high-value transactions count for more, we implement strict saturation caps. A billionaire cannot buy a perfect reputation with a single massive transaction; consistent history is mathematically required.|

We will provide further threat vectors and how we defend against these in due course.

---

## 5. Conclusion: The Utility of Probability

The Beta-Dirichlet Reputation System offers more than just a score; it offers **predictability**. By outputting a hierarchical trust score derived from the Expected Value of the posterior distribution, we provide consumers and smart contracts with a singular, actionable metric that contains deep historical context.

This system allows for:

- **Under-collateralised Lending:** Based on the statistical certainty of repayment.
- **Automated Vendor Selection:** Smart contracts autonomously selecting providers with the tightest confidence intervals.
- **Governance Weighting:** Voting power scaled not just by token holdings, but by proven beneficial behavior.

In the trustless environment of ERC-8004, we do not ask users to "trust." We give them the math to verify.

## 6. The Ensemble Horizon: A Hybrid Architecture

While the Beta-Dirichlet Engine provides the mathematical backbone of our trust assessment, we recognise that trust is a multi-dimensional phenomenon. A purely probabilistic model can sometimes be slow to react to "Black Swan" events or complex social collusion.

To mitigate this, the Beta-Dirichlet system acts as the "Core Logic," supported by a constellation of auxiliary modules. These modules do not replace the core probability distribution but act as **Signal Boosters** (amplifying relevant data) or **Circuit Breakers** (halting attacks).

### 6.1 The Graph-Aware Layer (Flow-Based Trust)

_Addressing the "Collusion" Vector._

The Beta-Dirichlet model excels at analysing individual history but treats agents as isolated nodes. To detect "Sybil Cliques" (where a group of fake agents artificially inflate each other's statistics), we overlay a **Transitive Flow Module** (inspired by EigenTrust/PageRank).

- **The Mechanism:** We construct a temporary "Web of Trust" graph during the update cycle.
- **The Integration:** The output of this flow analysis is not the final score. Instead, it serves as the **Confidence Scalar** for the _Consumer Trust Function_ $\tau(s_c)$.
- **The Benefit:** If an agent receives 1,000 positive reviews from a "cluster" of consumers who _only_ review each other, the Flow Module detects this closed loop. The trust mass flowing into the agent is throttled, preventing the localised probability update even if the math looks perfect on paper.

### 6.2 The Economic Layer (Game-Theoretic Stakes)

_Addressing the "Cost of Attack."_

Mathematics is powerful, but financial incentives are absolute. We incorporate a **"Skin-in-the-Game" (SitG)** module to handle the "Cold Start" problem more efficiently.

- **The Mechanism:** Agents can bypass the slow "Nursery" period by locking a cryptographic bond (Stake).
- **The Integration:** This stake does not buy reputation directly. Instead, it modifies the **Concentration Parameter** ($\eta$) of the agent's prior.
- **The Benefit:** A staked agent starts with a "tighter" probability curve. This means they gain trust faster if they perform well, but, crucially, their reputation collapses twice as fast if they defect. This introduces a "Volatility Risk" that makes it economically irrational to stake-and-scam.

### 6.3 The Uncertainty Layer (Dempster-Shafer Theory)

_Addressing the "Unknown Unknowns."_

Standard Bayesian approaches force a trade-off between "True" and "False." We integrate **Dempster-Shafer Theory (DST)** to explicitly model "Ignorance."

- **The Mechanism:** We separate the "plausibility" of an agent being good from the "belief" that they are good.
- **The Integration:** This is applied specifically to **High-Value Transaction Gating**.
- **The Benefit:** A new agent might have High Plausibility (no bad history), but Low Belief (no evidence). The system permits them to handle low-risk transactions (based on Plausibility) but blocks high-value interaction (requiring High Belief). This creates a tiered "security clearance" system that evolves organically.

### 6.4 The Semantic Layer (Fuzzy Logic Interface)

_Addressing "Human Readability."_

The raw output of a Dirichlet distribution (a vector of concentration parameters) is unintelligible to the average user.

- **The Mechanism:** We apply a **Fuzzy Inference System** to the output layer. We map the crisp numerical values of the Expected Value and Variance into linguistic variables (e.g., "High Reliability," "Volatile," "Suspicious").

- A crude example if output
    - `IF (History is Long) AND (Variance is Low) THEN (Status is "Verified Anchor")`
    - `IF (Value is High) AND (Recent Failures > 0) THEN (Status is "High Risk")`
- **The Benefit:** This defuzzification provides a user-friendly "Badge" system in the UI that is mathematically rigorous but functionally simple. It mitigates confusion and helps users make split-second decisions.

### 6.5 The Anomaly Layer (Bio-Inspired Defense)

_Addressing "Zero-Day Behavior."_

Malicious actors constantly evolve. A static rule set cannot catch a novel attack vector. We employ a **Negative Selection Algorithm** (Artificial Immune System).

- **The Mechanism:** The system generates random "detectors" (logic strings) that do not match known good behavior.
- **The Integration:** These detectors monitor the "Cluster Space." If an agent's trajectory through the state space matches a detector, it flags a **"Non-Self" Anomaly**.
- **The Benefit:** This triggers a "Probabilistic Freeze." The agent isn't banned, but their $\alpha$ and $\beta$ updates are paused pending a secondary review (or Oracle dispute). This prevents a novel exploit from draining reputation capital before a patch is issued.

---

## 7. Comparative Analysis: Why Ensemble Wins

By combining these architectures, we cover the blind spots inherent in any single approach:

| **Architecture**  | **Solo Weakness**                            | **Our Ensemble Solution**                                           |
| ----------------- | -------------------------------------------- | ------------------------------------------------------------------- |
| **Pure Bayesian** | Slow to trust; hard to detect collusion.     | **+ Flow Layer:** Detects graph-level collusion instantly.          |
| **Pure Staking**  | Plutocratic (rich = trusted).                | **+ Beta-Dirichlet:** Money only buys _volatility_, not score.      |
| **Pure ML/AI**    | "Black Box" opacity; requires training data. | **+ Fuzzy Logic:** Provides transparent, rule-based explainability. |
| **Web of Trust**  | Sybil vulnerable; path-dependent.            | **+ Economic Layer:** Sybils are expensive to create and maintain.  |

The Beta-Dirichlet system remains the **engine** - it provides the state. The ensemble layers are the **sensors and brakes** - ensuring that state remains accurate in a hostile, adversarial environment.
