# Algorithms To Live By: The Computer Science Of Human Decisions

## 1. Optimal Stopping: When to Stop Looking

### The Secretary Problem

* **Core dilemma of optimal stopping:** The fundamental question is not *which* option to choose, but *how long to search* before committing.
  * Applies broadly: hiring, renting, dating, driving, burglary, etc.
* **Problem setup (secretary problem):**
  * Goal: maximize the probability of selecting the single **best** option from $n$ candidates.
  * Candidates arrive in **random order**, observed sequentially.
  * Decision is **irreversible**: reject → permanently lost; accept → process ends.
  * Decision-maker has access only to **ordinal information** (relative ranking), not **cardinal values** (absolute scores).
* **Key result — the 37% Rule:**
  * Derived from the optimal stopping solution to the secretary problem.
  * Strategy: reject the first $\approx n/e \approx 37\%$ of candidates, then select the first candidate thereafter who is better than all seen so far.
  * Maximizes the probability of selecting the global best.

**Key takeaways:**

* **Optimal stopping** focuses on determining *when* to stop searching under uncertainty.
* The **secretary problem** formalizes decision-making with irreversible choices and ordinal information.
* The **37% ($1/e$) rule** provides a simple, provably optimal strategy for maximizing success probability.
* The problem’s history highlights how mathematical ideas propagate culturally and inform modern algorithmic thinking.

---

### Whence 37%?

* **Failure modes in optimal stopping:**
  * **Stopping too early:** Accepting before the global best appears.
  * **Stopping too late:** Rejecting the best while waiting for a nonexistent improvement.
  * Optimal policy balances these competing risks.
* **Structural constraint:**
  * Only **best-so-far** (record) candidates are ever admissible.
  * Probability that the $k$-th candidate is best-so-far is $1/k$, implying:
    * Early records are frequent but weak.
    * Later records are rare but strong.
* **Rejected intuitive heuristics:**
  * Accepting the $m$-th record (e.g., 3rd or 4th best-so-far).
  * Accepting after a long “drought” without records.
  * Neither maximizes success probability.
* **Optimal policy — Look-Then-Leap Rule:**
  * **Look phase:** Reject a fixed prefix of candidates unconditionally.
  * **Leap phase:** Accept the first candidate better than all seen in the look phase.
* **Small-$n$ analysis:**
  * $n=1$: Trivial success.
  * $n=2$: Maximum success probability $=1/2$, regardless of strategy.
  * $n=3$:
    * Optimal action hinges on candidate 2.
    * Strategy: accept candidate 2 iff she beats candidate 1.
    * Achieves success probability $=1/2 > 1/3$ (random choice).
  * $n=4$: Optimal to begin leaping after candidate 2.
  * $n=5$: Optimal to begin leaping after candidate 3.
* **Asymptotic result:**
  * Optimal cutoff converges to $n/e \approx 0.37n$.
  * **37% Rule:** Reject first $n/e$ candidates, then accept the next record.
* **Performance guarantee:**
  * Maximum probability of selecting the best converges to $1/e \approx 37\%$.
  * Notable symmetry: the **optimal cutoff fraction** and **success probability** are equal.
* **Implications of scale:**
  * Under random choice: success probability $=1/n \to 0$ as $n \to \infty$.
  * Under optimal stopping: success probability remains constant at $1/e$, independent of $n$.
  * Larger search spaces increase the **value of the optimal algorithm**, not the difficulty of the decision.

**Key takeaways:**

* Optimal stopping balances premature commitment against over-search.
* Only record-breaking options are ever rational to select.
* The **Look-Then-Leap** strategy is provably optimal.
* The **$1/e$ (37%) rule** governs both when to stop and the probability of success.
* Optimal stopping preserves performance even as the option set grows arbitrarily large.

---

### Lover’s Leap

* **Applying optimal stopping to romance (Michael Trick):**
  * Romantic partner search modeled as a **secretary problem**.
  * Unknown total $n$ replaced by a **time horizon** (ages 18–40).
  * Look-Then-Leap cutoff at $0.37 \times 22 \approx 8.1$ years → age **26.1**.
  * Trick proposed to the first partner better than all previous ones after this cutoff.
  * Model limitation exposed: **candidates can reject offers**.
* **Historical parallel — Johannes Kepler:**
  * Courted **11 women** after first wife’s death.
  * Preferred candidate #5 early on, but continued searching.
  * Ultimately returned to #5 and married her.
  * Illustrates violations of classical assumptions:
    * **Recall is possible**.
    * Searchers experience **regret, doubt, and fallback behavior**.
* **Limitations of the classical secretary problem:**
  * Assumes:
    * Guaranteed acceptance.
    * No recall of rejected candidates.
  * Real-world partner search violates both.
* **Variant with rejection allowed:**
  * If each proposal has acceptance probability $p=1/2$:
    * Optimal strategy: begin proposing after **25%** of the search.
    * Propose to **every best-so-far** candidate thereafter until accepted.
    * Probability of success (best candidate accepted): **25%**.
  * Earlier commitment compensates for rejection risk.
* **Variant with recall (second chances):**
  * If immediate proposals always succeed, but late proposals succeed with probability $1/2$:
    * Optimal **look phase** extends to **61%** of candidates.
    * **Leap** only for a best-yet candidate in remaining 39%.
    * If none accepted, **recall the best prior candidate**.
    * Success probability converges to **61%**, matching the cutoff.
* **Interpretive insight:**
  * **Restlessness and doubt** are not flaws but rational components of optimal strategies when recall is possible.
  * Optimal stopping adapts to structural features: rejection, recall, and timing asymmetries.

**Key takeaways:**

* The 37% Rule can be mapped from counts to time horizons.
* Real-world searches require extensions of the secretary problem.
* Allowing **rejection** shifts optimal commitment earlier.
* Allowing **recall** lengthens exploration and introduces fallback strategies.
* Optimal stopping explains emotional phenomena (doubt, hesitation) as algorithmically rational.

---

### Knowing a Good Thing When You See It: Full Information

* **Contrast with prior variants:**
  * Rejection and recall alter acceptance dynamics but preserve **ordinal-only information**.
  * These remain **no-information games**, still requiring a **Look-Then-Leap** structure.
* **No-information vs. full-information games:**
  * **No-information:** Only relative ranks are observable; magnitude of differences unknown.
    * Necessitates an initial calibration (“look”) phase.
  * **Full information:** Each applicant has an **objective, absolute score** (e.g., percentile).
    * Eliminates the need for calibration.
* **Assumptions of full information:**
  * Applicant pool is representative of the population.
  * A single measurable criterion determines quality.
  * Scores directly encode probabilities (e.g., 96th percentile ⇒ $P(\text{better}) = 1/20$).
* **Decision rule — Threshold Rule:**
  * Immediately accept an applicant if her score exceeds a **dynamic percentile threshold**.
  * Threshold depends solely on the **number of remaining applicants**, not past observations.
  * Still never accept a non–best-so-far candidate.
* **Backward induction logic:**
  * Last applicant: must accept.
  * Second-to-last: accept if above 50th percentile.
  * Third-to-last: accept if above 69th percentile.
  * Fourth-to-last: accept if above 78th percentile.
  * General principle: be **more selective** when many options remain; **lower thresholds** as options shrink.
* **Implications for standards:**
  * “Lower your standards” when few options remain; “raise them” when many remain.
  * Full information specifies **exactly how much** to adjust thresholds.
* **Performance comparison:**
  * Probability of selecting the best applicant:
    * No-information game: $1/e \approx 37\%$.
    * Full-information game: **58%**.
  * Full information yields success more often than failure, even as $n \to \infty$.
* **Interpretive insight:**
  * Objective metrics (e.g., income percentile, test scores) outperform subjective criteria that require calibration.
  * Any population-relative metric transforms the problem from Look-Then-Leap to Threshold-based stopping.
* **Broader applicability:**
  * Sequential decision problems: selling houses, parking, gambling, quitting strategies.
  * Many such problems admit known optimal stopping solutions.

**Key takeaways:**

* The classical secretary problem is a **no-information game**.
* Full information eliminates the need for an exploratory phase.
* Optimal strategy shifts from **Look-Then-Leap** to a **dynamic Threshold Rule**.
* Backward induction determines exact acceptance cutoffs.
* Full information raises success probability from 37% to **58%**.

---

### When to Sell

* **Problem shift from dating to real estate:**
  * Selling a house is an **optimal stopping problem with costs**.
  * Offers arrive sequentially; rejecting an offer incurs a **waiting cost** (e.g., mortgage, time).
* **Relation to full-information games:**
  * Offers have **objective dollar values** (cardinal information).
  * Market knowledge provides a known **distribution** (percentile interpretation).
  * Objective is **not** to pick the single best offer, but to **maximize expected net payoff**.
* **Key modeling assumptions (simple case):**
  * Known minimum and maximum offer values.
  * Offers are **i.i.d. uniform** over this range.
  * No risk of offers or savings running out.
* **Optimal strategy — Fixed Threshold Rule:**
  * Set a **reservation price** *before* search begins.
  * Reject all offers below it; accept the first offer above it.
  * Threshold depends **only on the cost of waiting**, not on elapsed time or past offers.
* **Cost–benefit logic:**
  * Rejecting an offer is optimal iff:
    * $(\text{expected gain from waiting}) > (\text{cost of waiting})$.
  * Produces a closed-form stopping threshold as a function of:
    * Offer range $(\max - \min)$.
    * Cost per additional offer.
* **Numerical illustration ($400k–$500k range):**
  * Waiting cost = $1: hold out for $499,552.79.
  * Waiting cost = $2,000: threshold = $480,000.
  * Waiting cost = $10,000: threshold = $455,279.
  * Waiting cost > $50,000 (half the range): Optimal to accept the **first offer**.
* **Key structural insight:**
  * Since probabilities and costs are stationary, the threshold **never declines over time**.
  * Unlike dating or recall-enabled problems, there is no reason to “lower standards” as search continues.
* **Empirical validation:**
  * Laura Albert McLay applied this reasoning in selling her own house.
  * Mathematical confidence helped withstand short-term regret and noise.
* **Broader applications:**
  * Job search models explain coexistence of unemployment and vacancies.
  * Applies whenever offers arrive sequentially and searching has a cost.
* **Recall is irrelevant (and harmful):**
  * Even if earlier offers remain available, **never go back**.
  * Past waiting costs are **sunk costs**; thresholds do not change.
  * If an offer wasn’t good enough then, it isn’t good enough now.

**Key takeaways:**

* Selling with costs is a **full-information optimal stopping problem**.
* Optimal policy is a **fixed reservation price**, set ex ante.
* Threshold depends only on **search cost and offer range**, not time.
* High search costs justify early acceptance; low costs justify patience.
* Ignore sunk costs and never reconsider rejected offers.

---

### When to Park

* **Parking as an optimal stopping problem:**
  * Sequential decisions under irreversible motion (once you pass a spot, you usually don’t go back).
  * Applies to restaurants, bathrooms, but most sharply to **urban parking**.
* **Key variables in parking choice (Shoup’s model):**
  * Monetary cost of the space.
  * Walking distance to destination.
  * Search time and fuel burned.
  * Number of passengers (split price, not time).
  * Strategic interaction with other drivers (**game-theoretic competition**).
* **Occupancy rate as the dominant parameter:**
  * Defined as the proportion of parking spots currently filled.
  * Low occupancy → easy search; high occupancy → long cruising and congestion.
  * Near-full occupancy creates disproportionate harm:
    * Increasing occupancy from 90% to 95% accommodates only 5% more cars but **doubles search time**.
* **Policy diagnosis and solution (Donald Shoup):**
  * Underpriced or free parking drives occupancy toward ~100%.
  * Leads to cruising, wasted fuel, pollution, and congestion.
  * Solution: **demand-responsive pricing** via digital meters.
    * Target occupancy ≈ **85%**.
    * Implemented in places like downtown San Francisco.
* **Parking strategy under optimal stopping:**
  * Modeled as driving along an infinite street with evenly spaced spots.
  * Objective: minimize walking distance.
  * Optimal policy: **Look-Then-Leap Rule**.
    * Pass up spots beyond a cutoff distance.
    * Take the first vacant spot after that point.
  * Cutoff distance depends solely on **occupancy rate**.
* **Quantitative implications:**
  * At 99% occupancy (1% vacant):
    * Start accepting spots ≈ **70 spaces** (~1/4 mile) from destination.
  * At 85% occupancy:
    * Start looking seriously only about **half a block** away.
* **Robustness of results:**
  * Extensions studied:
    * Allowing U-turns.
    * Decreasing supply closer to destination.
    * Competition among multiple drivers.
  * Qualitative result holds universally: **more vacancies reduce search cost**.
* **Policy-level insight:**
  * Parking is not just a utilization problem but a **process problem**.
  * Optimal policy minimizes total system cost (time, fuel, pollution), not spot occupancy.
  * Counterintuitively, **empty spots on popular streets signal success**, not failure.
* **Practical epilogue:**
  * Even parking experts avoid the stopping problem entirely when possible—by not driving.

**Key takeaways:**

* Parking search is a canonical **optimal stopping problem**.
* **Occupancy rate** determines both individual strategy and system-wide congestion.
* Optimal parking follows a **distance-based Look-Then-Leap rule**.
* Extremely high occupancy is socially inefficient.
* Smart pricing that preserves vacancies improves outcomes for everyone.

---

### When to Quit

* **Motivating case — Boris Berezovsky:**
  * Former mathematician turned oligarch; rapid ascent via sequential, compounding opportunities.
  * Political engagement increased both rewards and existential risk.
  * Illustrates the real-world question: **when to quit while ahead**.
* **Quitting as an optimal stopping problem:**
  * Modeled via the **burglar problem**:
    * Sequential actions yield rewards.
    * Each action has a fixed probability of success and a probability of catastrophic failure.
    * Failure wipes out all accumulated gains.
* **Optimal policy (burglar problem):**
  * Let $p$ = probability of success, $q = 1 - p$ = probability of failure.
  * Optimal number of attempts is approximately: $`\text{\# attempts} \approx \frac{p}{q}`$
  * Examples:
    * Skilled burglar ($p=0.9$, $q=0.1$): quit after $9$ attempts.
    * Amateur ($p=0.5$, $q=0.5$): stop after $1$ attempt.
  * Strategy maximizes **expected total reward**, not probability of survival.
* **Narrative irony:**
  * Berezovsky authored the only book devoted entirely to the secretary problem.
  * Despite understanding optimal stopping, he did not stop early enough in practice.
* **Psychological counterpoint:**
  * Anecdote depicts Berezovsky as pathologically unwilling to quit.
  * Highlights tension between **algorithmic optimality** and human temperament.
* **No-optimal-stopping scenarios:**
  * Some sequential problems admit **no optimal stopping rule**.
  * Example: **Triple-or-nothing game**.
    * Start with $1.
    * Each round: 50% chance to triple stake, 50% chance to lose everything.
    * Expected value increases each round:
      * $1 → expected $1.50 → expected $4.50 → …
  * Mathematical result:
    * Expected value strictly increases with continued play.
    * Optimal strategy in expectation: **never stop**.
    * Almost sure outcome: **eventual ruin**.
* **Normative conclusion:**
  * Maximizing expected value can be incompatible with acceptable risk.
  * Some problems are **pathological**: optimal play guarantees disaster with probability 1.
  * Rational response is **avoidance**, not optimization.

**Key takeaways:**

* “Quit while you’re ahead” can be formalized via optimal stopping.
* In risk-of-ruin problems, optimal quitting depends on the **success-to-failure ratio**.
* Expected-value maximization may recommend strategies that are practically suicidal.
* Some sequential games have **no safe optimal stopping rule**.
* Not all problems should be solved—some should be refused.

---

### Always Be Stopping

* **Ubiquity of optimal stopping:** Daily decisions (hiring, dating, housing) structurally mirror **optimal stopping problems**, raising the question of whether humans follow theoretically optimal strategies.
* **Empirical findings:**
  * Multiple laboratory studies show **systematic early stopping**, with participants terminating search before the optimal point.
  * In Rapoport & Seale’s secretary-problem experiments (1990s, $n=40$ or $80$ applicants):
    * Success rate ≈ **31%**, compared to the optimal **37%**.
    * Behavior broadly matched the **Look-Then-Leap Rule**, but **>80% of participants leapt too early**.
* **Interpretation via search costs:**
  * Classical secretary models assume **zero cost of search**.
  * Introducing even a small per-sample cost (e.g., **1% of the value of the best outcome per applicant**) shifts the optimal stopping threshold to match observed human behavior.
* **Endogenous time costs:**
  * Even when experiments impose no explicit cost, humans experience **implicit time costs** (opportunity cost, impatience, boredom).
  * These **endogenous costs** are typically excluded from formal models but plausibly explain deviations from theoretical optima.
* **Modeling limitation:**
  * Human factors like **boredom** are not irrational but are difficult to formalize rigorously within optimal stopping frameworks.
* **Broader implication:**
  * Time’s irreversibility (strict seriality, no recall) is not an artificial assumption of the secretary problem but a fundamental property of real life.
  * All decisions reduce to choosing **when** to act under uncertainty, accepting that:
    * Optimal play can still imply high failure rates.
    * **Inaction is as irreversible as action**.
* **Managerial insight:**
  * Real-world rationality cannot rely on exhaustive evaluation; under time pressure, **the critical decision variable is the stopping time**, not just option quality.

**Key takeaways:**

* **Humans stop early:** Empirical behavior systematically deviates from zero-cost optimal stopping.
* **Implicit time costs matter:** Endogenous costs align observed behavior with modified optimal strategies.
* **Optimal stopping is universal:** Time’s one-way nature turns nearly all decisions into stopping problems.
* **Managerial focus:** Effective decision-making hinges on calibrating *when to stop*, not merely *what to choose*.

---

## 2. Explore/Exploit: The Latest vs. the Greatest

### Explore/Exploit

* **Definitions (CS meaning):**
  * **Exploration:** Acquiring information about unknown options.
  * **Exploitation:** Leveraging known information to obtain reliable rewards.
  * The terms are value-neutral in computer science, unlike their moral connotations in everyday language.
* **Tradeoff intuition:**
  * Pure exploitation is suboptimal (never learning).
  * Pure exploration is also costly (never enjoying accumulated knowledge).
  * Many valued life experiences (traditions, favorites, mastery) are instances of **exploitation**.
* **Costs of over-exploration:**
  * Constant exploration can prevent enjoyment and consolidation of expertise.
  * Example: music journalism forces maximal exploration, crowding out exploitation of known favorites.
* **Formal model — Multi-Armed Bandit Problem:**
  * Setup: Multiple options (“arms”), each with unknown payoff distributions.
  * Goal: Maximize cumulative reward over time by balancing:
    * Sampling uncertain options (**explore**),
    * Favoring empirically strong options (**exploit**).
* **Uncertainty vs. expected value:**
  * Naïve strategy: choose the arm with highest empirical mean payoff.
    * Example:
      * Arm A: 9 wins / 15 pulls → empirical value $0.6$.
      * Arm B: 1 win / 2 pulls → empirical value $0.5$.
  * Problem: empirical means ignore **uncertainty**; sparse data implies high variance and latent upside.
* **Sequential decision context:**
  * Decisions are not independent; they recur over time.
  * Optimal choice depends on future opportunities to exploit information gained now.
  * Expected value maximization for a single step is insufficient.
* **Broader implications:**
  * Applies to personal choice (restaurants, music), organizational design, web optimization, and clinical trials.
  * As Whittle notes, the bandit problem captures a core conflict in all human action.
* **Critical dependency:**
  * Optimal explore/exploit balance depends on the **time horizon**—how many decisions remain.
  * Without knowing how long the process lasts, the “best” action is undefined.

**Key takeaways:**

* **Explore/exploit is a central decision tradeoff:** Learning vs. earning.
* **Uncertainty matters:** Sparse data can justify exploration even with lower empirical payoff.
* **Decisions are coupled over time:** Long-term payoff dominates single-step optimality.
* **Time horizon is decisive:** Optimal strategy depends on how long you expect to keep choosing.

---

### Seize the Interval

* **Core tension:** “Carpe diem” is interval-dependent.
  * Seizing *a day* (short horizon) vs. seizing *a lifetime* (long horizon) imply opposite strategies.
  * Explore/exploit balance is meaningless without specifying the **planning interval**.
* **Interval determines value of actions:**
  * **Exploration value is decreasing over time:**
    * Discovering a new option late leaves little opportunity to exploit it.
    * Formally: the expected future reward of exploration shrinks as the remaining horizon approaches zero.
  * **Exploitation value is increasing over time:**
    * The best-known option monotonically improves or stays constant as information accumulates.
* **Heuristic policy implied:**
  * **Explore early**, when there is time to amortize learning.
  * **Exploit late**, when the opportunity cost of exploration exceeds its informational value.
  * Summary rule: *the interval makes the strategy*.
* **Empirical anecdote (Stucchio):**
  * When arriving in a city (long horizon): aggressive exploration.
  * When preparing to leave (short horizon): repeated exploitation of known favorites.
  * Even potentially superior discoveries are not worth the risk if future use is limited.
* **Inference reversal:**
  * Observing behavior allows inference of perceived interval.
  * Heavy exploitation signals belief in a **short remaining horizon**.
* **Industry-scale example — Hollywood:**
  * Rising dominance of sequels:
    * 1981: 2/10 top-grossing films were sequels.
    * 1991: 3/10.
    * 2001: 5/10.
    * 2011: 8/10.
  * Sequels = **exploitation** (known IP, lower uncertainty, guaranteed audience).
  * Declining exploration implies studios optimizing for short-term payoff.
* **Economic drivers:**
  * Studio profits declined ~40% (2007–2011).
  * Ticket sales fell in 7 of the previous 10 years.
  * Rational response under shrinking margins: pull the arms with the highest known expected value.
* **Strategic implication:**
  * Excessive exploitation crowds out the creation of future high-value options.
  * A system dominated by exploitation is efficient locally but fragile globally.

**Key takeaways:**

* **Time horizon is the dominant variable** in explore/exploit strategy.
* **Explore early, exploit late** is optimal when learning compounds.
* **Over-exploitation signals short-termism** and declining confidence in the future.
* **Managerial insight:** declining exploration today predicts scarcity of high-value options tomorrow.

---

### Win-Stay

* **Computational difficulty of bandits:**
  * Exact optimal policies for the **multi-armed bandit problem** are notoriously hard to derive.
  * Peter Whittle recounts WWII-era analysis exhaustion, highlighting the problem’s analytical intractability.
* **Robbins’s contribution (1952):**
  * Herbert Robbins analyzed the **two-armed bandit** case.
  * Proposed **Win-Stay, Lose-Shift (WSLS)**:
    * Choose an arm at random.
    * **Win → Stay:** keep pulling the same arm after a payoff.
    * **Lose → Shift:** switch arms after a failure.
  * Proven to perform **better than chance**, though not optimal.
* **Interpretation of win-stay:**
  * Bayesian intuition: a payoff updates posterior belief upward.
  * “Stay on a winner” is a **structural component of optimal policies** under many conditions.
* **Problems with lose-shift:**
  * Overreacts to noise: a single failure causes abandonment of otherwise high-quality options.
  * Violates robustness: good stochastic options are imperfect by nature.
* **Missing interval awareness:**
  * WSLS ignores the **optimization horizon**.
  * Always switching after failure is irrational when the remaining interval is short (e.g., last opportunity).
* **Bellman’s exact solution:**
  * Richard Bellman solved the bandit problem for cases with:
    * Known number of options.
    * Known finite number of pulls.
  * Method: **dynamic programming / backward induction**.
    * Compute optimal action at the final step.
    * Recursively solve earlier steps.
* **Limits of Bellman’s approach:**
  * Computationally explosive with many arms or long horizons.
  * Requires full knowledge of future opportunities and options.
* **State of the field:**
  * Despite partial results, the general bandit problem remained unsolved for decades.
  * Became a canonical example of **decision-theoretic hardness** (“a byword for intransigence”).

**Key takeaways:**

* **Win-stay** is a robust and widely optimal heuristic component.
* **Lose-shift** is overly myopic and noise-sensitive.
* **Interval awareness is essential** for rational explore/exploit behavior.
* **Exact optimality is computationally infeasible** without strong assumptions.
* The bandit problem exemplifies the limits of algorithmic decision-making under uncertainty.

---

### The Gittins Index

* **From applied problem to general solution:**
  * In the 1970s, **John Gittins**, working for Unilever on drug-trial optimization, reframed the problem as a general **multi-armed bandit** with uncertain rewards.
  * Core elements: multiple options, stochastic payoffs, resource allocation over time → classic explore/exploit tradeoff.
* **Role of discounting:**
  * Unlike fixed-horizon models, Gittins assumed an **indefinite future with discounted rewards**.
  * **Discounting:** present rewards are valued more than future ones.
    * Assumed **geometric discounting**: each future payoff is multiplied by a constant factor $\gamma \in (0,1)$.
    * Example: if $\gamma = 0.99$, tomorrow’s payoff is worth 99% of today’s.
* **Key conceptual move (indexability):**
  * Treat each arm **independently**.
  * For each arm, compute the **minimum guaranteed payout rate** that would make you indifferent between:
    * Continuing to sample that arm.
    * Taking the sure thing and never sampling it again.
  * This value is the **Gittins index** (originally “dynamic allocation index”).
* **Optimal strategy:**
  * **Always select the arm with the highest Gittins index**.
  * This strategy is **provably optimal** for multi-armed bandits with geometrically discounted rewards.
  * Exploration and exploitation collapse into a single scalar comparison.
* **Properties of the Gittins index:**
  * **Scales independently of the number of arms** (each arm indexed separately).
  * Encodes:
    * Expected payoff.
    * Uncertainty (information value).
    * Discounting of future opportunities.
* **Illustrative implications (from tables):**
  * With $\gamma = 0.90$:
    * Arm with record **1–1** (EV = 0.50) has index **0.6346**.
    * Arm with record **9–6** (EV = 0.60) has index **0.6300**.
    * ⇒ Prefer the less-tested arm (exploration bonus).
  * **Win-stay behavior:** after a win, index strictly increases.
  * **Lose-shift is suboptimal:** e.g., 9 wins then 1 loss still yields a high index (0.8695).
* **Value of the unknown:**
  * An untested arm (**0–0**) has:
    * EV = 0.5000.
    * Index = **0.7029** (for $\gamma = 0.90$).
  * Exploration bonus declines slowly as evidence accumulates.
  * Even early failures leave the index above EV.
* **Effect of heavier future weighting:**
  * With $\gamma = 0.99$:
    * Exploration becomes dramatically more valuable.
    * A 0–0 arm is equivalent to a guaranteed **86.99%** payout.
  * Longer effective horizons favor novelty and experimentation.
* **Interpretation:**
  * The index formally explains why **the unknown can rationally dominate the known**, even when expected values are equal.
  * Exploration has intrinsic value because it creates future exploitation opportunities.
* **Limitations:**
  * Optimality depends on:
    * **Geometric discounting** (often violated in human behavior).
    * **No switching costs** between options.
  * **Computational burden:** difficult to calculate in real time without precomputed tables.
* **Aftermath:**
  * Motivated search for **simpler, approximate bandit algorithms** that are:
    * Easier to compute.
    * More behaviorally realistic.
    * Nearly as effective in practice.

**Key takeaways:**

* **Gittins index** provides an exact solution to discounted multi-armed bandits.
* **Always pick the arm with the highest index** under geometric discounting.
* **Exploration has quantifiable value** due to information and future payoff.
* **Unknown options can rationally dominate known ones** early on.
* Strong assumptions limit direct applicability, motivating simpler heuristics.

---

### Regret and Optimism

* **Alternative to Gittins index: minimize regret**
  * When geometric discounting is inappropriate or computation is infeasible, optimize for **regret**.
  * **Regret (formal definition):**
    * The difference between:
      * Total reward obtained by a given strategy.
      * Total reward that would have been obtained by always choosing the best arm (known in hindsight).
    * Measures Barnard’s “loss of what might have been.”
  * Regret reframes exploration as a cost of ignorance relative to perfect foresight.
* **Properties of regret (Lai & Robbins, 1985):**
  * Assuming non-omniscience:
    * **Regret grows without bound** over time (mistakes are inevitable).
    * Good strategies cause regret to grow **more slowly** than bad ones.
    * The **optimal achievable growth rate** of regret is **logarithmic** in the number of pulls.
  * **Logarithmic regret implications:**
    * As many mistakes in first 10 trials as in the next 90.
    * As many in first year as in the rest of the decade.
    * Error rate declines over time.
  * Thus, perfection is impossible, but **regret accumulation decelerates** under optimal strategies.
* **Upper Confidence Bound (UCB) algorithms:**
  * Developed to achieve minimal (logarithmic) regret.
  * Core rule:
    * For each arm, compute:

      ```text
      UCB = estimated mean + uncertainty bonus
      ```

    * Choose the arm with the highest **upper confidence bound**.
  * The “uncertainty bonus” shrinks as data accumulates.
  * Arms with fewer samples receive larger bonuses.
* **Conceptual principle: “Optimism in the face of uncertainty”**
  * Act as if each option is as good as it plausibly could be.
  * Exploration emerges naturally:
    * New/rarely tried options look promising because their confidence intervals are wide.
    * Heavily sampled mediocre options lose appeal as their intervals tighten.
    * (A restaurant with a single mediocre review still retains a *potential* for greatness that’s absent in a restaurant with hundreds of such reviews.)
  * Similar to Gittins:
    * Both assign a scalar index to each arm.
    * Both embed exploration into the score.
  * Advantages over Gittins:
    * Computationally simpler.
    * No assumption of geometric discounting.
    * Explicit regret guarantees.
* **Behavioral interpretation:**
  * UCB formalizes the **benefit of the doubt**.
  * Encourages:
    * Trying new restaurants.
    * Meeting new people.
    * Exploring unfamiliar paths.
  * Optimism is not naïveté—it is statistically justified when uncertainty is high.
* **Structural insight:**
  * Expected value alone is insufficient.
  * Decision score must include:
    * Exploitation term (mean reward).
    * Exploration term (uncertainty).
  * Exploration weight declines over time → convergence toward exploitation.

**Key Takeaways:**

* **Regret** = loss relative to perfect hindsight.
* Minimal achievable regret grows **logarithmically**.
* **Upper Confidence Bound** algorithms achieve this bound.
* UCB selects the arm with the highest plausible payoff, not the highest observed average.
* Rational optimism systematically prevents long-term regret.
* Over time, uncertainty shrinks and behavior naturally shifts toward exploitation.

---

### Bandits Online

* **A/B testing = operationalized explore/exploit**
  * Randomly assign users to different webpage variants (colors, headlines, layouts).
  * Measure performance (click-through rate, revenue per visitor).
  * After statistical significance, deploy the “winner” or iterate further.
  * Canonical form: 50/50 traffic split → fixed test period → commit to best arm.
* **Obama 2008 campaign (Dan Siroker)**
  * Applied A/B testing to donation page → **+$57M in donations**.
  * Segment-specific optimization:
    * First-time visitors: **“DONATE AND GET A GIFT”** (even net of gift cost).
    * Newsletter subscribers (never donated): **“PLEASE DONATE”** (guilt framing).
    * Prior donors: **“CONTRIBUTE”** (semantic reframing).
    * Best image: simple black-and-white Obama family photo.
  * Demonstrates **contextual bandits**: different subpopulations require different arms.
* **Internet as large-scale bandit experiment**
  * Since ~2000, firms (Google, Amazon, Facebook) continuously test:
    * UI navigation, email timing/subject lines, pricing, algorithms.
    * e.g., Google’s **41 shades of blue** experiment.
  * Massive economic stakes:
    * > 90% of Google’s ~$50B revenue (at the time) from ads.
    * Hundreds of billions in online commerce.
  * Users are the **environment being optimized over**—“you’re the jackpot.”
* **Algorithmic refinement beyond naïve A/B**
  * Fixed 50/50 split wastes reward: half of users receive inferior arm during test.
  * Implies inefficiency in regret terms:
    * Cumulative regret grows while inferior arm is still allocated traffic.
  * Incentive to use adaptive allocation (e.g., bandit algorithms) that:
    * Shift traffic toward higher-performing arms over time.
    * Reduce regret while maintaining exploration.
* **Strategic implications**
  * Explore/exploit is no longer abstract theory—it powers core digital infrastructure.
  * Algorithm choice materially affects:
    * Revenue
    * Political outcomes
    * Potentially life-critical domains (implied extension beyond commerce).
  * Fine distinctions among algorithms (UCB, Thompson sampling, etc.) are economically and socially consequential.

**Key takeaways:**

* **A/B testing is a practical multi-armed bandit implementation** in online systems.
* Canonical fixed-split testing incurs **avoidable regret**.
* Adaptive explore/exploit algorithms drive a significant fraction of the Internet economy.
* Contextual segmentation (different arms for different users) greatly amplifies impact.
* Algorithm design choices have large-scale economic—and potentially human—consequences.

---

### Clinical Trials on Trial

* **Tuskegee Syphilis Study (1932–1972)**
  * Hundreds of African-American men deliberately untreated.
  * Exposure (1972) → Belmont Report (1979): formalized research ethics.
  * Core tension: **Hippocratic “do no harm” vs. need to learn what is harmful**.
  * **Beneficence conflict**: short-term patient risk vs. long-term societal benefit.
* **Standard Randomized Clinical Trials (RCTs)**
  * Fixed randomization: patients split into treatment arms for full duration.
  * Rarely stopped early.
  * Objective: maximize **statistical certainty**, not patient-level outcomes during trial.
  * Structurally identical to fixed A/B testing → some patients knowingly receive inferior treatments.
  * Raises regret-like concern: accumulating avoidable harm while evidence emerges.
* **Adaptive Trials as Multi-Armed Bandits**
  * Proposal: treat treatment allocation as **bandit problem**—shift probability toward better-performing arms during trial.
  * **Marvin Zelen (1969)**: randomized “play-the-winner” (Win-Stay, Lose-Shift variant).
    * Urn model:
      * Start: 1 ball per treatment.
      * Success → add ball of same treatment.
      * Failure → add ball of alternative treatment.
    * Allocation probability ∝ historical success.
* **ECMO Case Study**
  * ECMO: external oxygenation for infants with respiratory failure; high risk but potentially lifesaving.
  * **Michigan trial (1982–84, adaptive design)**:
    * 1 conventional → death.
    * 11 ECMO in a row → all survived.
    * Post-trial: 8 ECMO → 8 survived; 2 conventional → 2 died.
  * Controversy:
    * Small control group → statistical skepticism.
    * Critics (e.g., Jim Ware) argued evidence insufficient for routine adoption.
* **Ware Trial (Modified Adaptive Design)**
  * Randomize until prespecified deaths in one arm → then switch all to superior treatment.
  * Phase 1:
    * 4/10 conventional deaths.
    * 0/9 ECMO deaths → trigger switch.
  * Phase 2:
    * 20 ECMO → 19 survived.
  * Conclusion: further randomization ethically indefensible.
* **Ongoing Ethical Dispute**
  * **Don Berry (bandit expert)**: even Ware’s continued randomization unethical.
  * UK RCT (1990s, traditional 50/50 split, ~200 infants):
    * ECMO superior but less dramatic.
    * **24 additional deaths** in conventional group.
  * Core issue: institutional trust in fixed RCT methodology vs. adaptive statistical innovation.
* **Institutional Dynamics**
  * 20th-century statistics provided medicine with standardized evidentiary norms.
  * Adaptive designs challenge entrenched methodological legitimacy.
  * Berry later applied bandit-based designs in oncology.
  * FDA draft guidance (2010, 2015) on **Adaptive Design trials** signals gradual mainstream acceptance.
* **Strategic Framing**
  * Clinical trials instantiate the explore/exploit tradeoff under life-or-death stakes.
  * Fixed randomization maximizes inferential clarity.
  * Adaptive allocation minimizes cumulative regret (avoidable harm) but may reduce statistical orthodoxy and perceived robustness.

**Key takeaways:**

* Clinical trials face a formal **explore vs. exploit ethical dilemma**.
* Standard RCTs optimize knowledge; adaptive designs optimize patient outcomes during learning.
* **Play-the-winner (urn models)** operationalize bandit principles in medicine.
* ECMO controversy illustrates tradeoff between statistical certainty and minimizing harm.
* Regulatory institutions are slowly shifting toward **adaptive, bandit-inspired methodologies**.

---

### The Restless World

* **Bandits as a general decision model**
  * Most real-world decisions are sequential and informational: outcomes update beliefs and affect future choices.
  * Key question: how well do humans solve **explore/exploit** tradeoffs?
* **Systematic over-exploration in humans**
  * **Tversky & Edwards (1966)**: two-light experiment (unknown fixed probabilities), 1,000 rounds of either:
    * **Observe** (pure exploration), or
    * **Bet** (pure exploitation; payoff revealed only at end).
  * Example: 60% vs 40% light.
    * Subjects observed **505 times** on average.
    * Optimal strategy: switch to betting after ~**38 observations**, leaving 962 exploitation opportunities.
  * Conclusion: humans gather far more information than is payoff-maximizing.
* **Airline choice experiment (Meyer & Shi, 1990s)**
  * One airline with known on-time rate; one unknown.
  * Optimal policy (finite horizon, static payoffs):
    * Initially **fully explore** the unknown airline.
    * If its **Gittins index** falls below the known airline’s rate, **switch permanently** (no further information available once abandoned).
  * Observed behavior:
    * Under-used good new airline; over-used bad one.
    * Failed to make decisive switches; alternated excessively.
  * Again consistent with **over-exploration**.
* **Four-armed bandit study (Steyvers, Lee, Wagenmakers)**
  * 15 pulls; strategies classified:
    * 30% near-optimal.
    * 47% resembled **Win-Stay, Lose-Shift**.
    * 22% quasi-random between novelty and current best.
  * Late-stage behavior showed excessive deviation from pure exploitation.
* **Asymmetry in human errors**
  * Earlier: commit too soon (secretary problem).
  * Here: explore too long.
  * Insight: exploration bias may be adaptive if environment is non-stationary.
* **Restless Bandits (Non-Stationary Payoffs)**
  * Classical assumption: arm payoffs are fixed over time.
  * Real world: probabilities drift (airlines improve/decline, restaurants change management).
  * When payoff probabilities evolve over time:
    * Problem becomes a **restless bandit**.
    * No tractable optimal solution; believed computationally intractable.
    * Exploration cannot cease permanently—learning must be ongoing.
  * Example: revisit previously bad restaurant; world may have changed.
* **Practical implications**
  * In non-stationary environments:
    * Continued exploration can be rational.
    * Over-exploration may hedge against change.
  * In relatively static environments:
    * Classical tools (Gittins index, UCB) provide strong approximations.
    * Modern industrial standardization (“A Coke is a Coke”) reduces environmental volatility.
* **Conceptual contributions of classical bandits**
  * Clarifies:
    * **Explore vs. exploit tension**
    * Importance of **time horizon / interval**
    * Value of the **0–0 option** (unknowns)
    * **Regret minimization** as a performance metric
  * Even when optimal policies are infeasible (restless case), the framework structures strategic thinking across domains.

**Key takeaways:**

* Humans systematically **over-explore** relative to optimal policies in stationary settings.
* Optimal strategies often require **early concentrated exploration** followed by decisive exploitation.
* In **restless (non-stationary) environments**, continued exploration is rational; no fully optimal algorithm exists.
* Classical bandit concepts remain powerful **heuristics and managerial lenses**, even when exact optimization is impossible.

---

### Explore …

* **Long-interval bandits:** Many real decisions (career paths, relationships, worldview formation) have horizons too long for lab replication. Over a lifetime, the optimal structure resembles **early exploration → later exploitation**.
* **Development as algorithmic design (Gopnik):**
  * Humans’ extended childhood functions as a **dedicated exploration phase**.
  * In multi-armed bandits, optimal policies front-load exploration, accepting low immediate payoff to improve long-run reward.
  * Exploration has short-term cost (low payoff); childhood externalizes that cost to caregivers.
  * Thus, evolution implements an **intertemporal tradeoff solution**: protected exploration early, payoff harvesting later.
* **Reframing child “irrationality”:**
  * Traditional view: children are deficient at exploitation (planning, focus, delayed gratification).
  * Bandit view: they are optimized for exploration:
    * Random button pressing.
    * Rapid task-switching.
    * Fascination with novelty.
  * Baby putting objects in mouth ≈ systematically “pulling all arms” to map payoff distributions.
* **Rationality over multiple decisions:**
  * If each decision were terminal → pure exploitation is optimal.
  * Over many sequential decisions → exploration is instrumentally rational.
  * Early-life emphasis on:
    * Novel over best,
    * Exciting over safe,
    * Random over optimized,
      improves long-run regret minimization.
* **Managerial analogy:**
  * Early career / early venture stage → tolerate variance and experimentation.
  * Later stage → consolidate around highest-performing arms.
  * Misjudging interval length leads to premature exploitation.
* **Normative shift:**
  * Human intuitions overweight immediate payoff (exploit bias).
  * Proper lifetime optimization requires staged strategy:
    * Phase 1: information acquisition.
    * Phase 2: reward extraction.

**Key takeaways:**

* **Childhood = exploration phase** of a lifelong bandit algorithm.
* **Exploration is rational when the decision horizon is long.**
* Apparent inefficiency (randomness, distraction) may be optimal under information acquisition objectives.
* Optimal life strategy: **front-load exploration, back-load exploitation**.
* Rationality must be evaluated over the full interval, not single decisions.

---

### … And Exploit

* **Aging as interval compression:**
  * As perceived remaining horizon shrinks, optimal policy shifts from **exploration → exploitation**.
  * Social network contraction with age reflects rational interval-aware strategy, not decline.
* **Carstensen’s Socioemotional Selectivity Theory:**
  * Older adults **strategically prune** peripheral ties.
  * Objective: maximize **emotional payoff**, minimize risk.
  * Networks shrink by deliberate selection toward high-value, high-certainty relationships (core friends/family).
* **Experimental evidence (interval manipulation):**
  * Choice: 30 minutes with
    * Family member,
    * Book author,
    * New acquaintance.
  * Older adults → prefer family (exploit known payoff).
  * Young adults → indifferent; open to novelty (explore).
  * If young imagine moving away (shortened interval) → shift to family.
  * If elderly imagine +20 years life extension → shift toward exploratory preferences.
  * Conclusion: behavior depends on **perceived time horizon**, not chronological age.
* **Algorithmic interpretation:**
  * With long horizon → exploration term (e.g., UCB bonus, Gittins index inflation) dominates.
  * With short horizon → uncertainty bonus collapses; exploit highest expected value arm.
  * Elderly are behaving optimally under shorter intervals.
* **Environmental transitions:**
  * College (early life, exploration phase) → new environment is exciting.
  * Retirement home (late life, exploitation phase) → new environment is costly; disrupts optimized portfolio of relationships.
* **Intergenerational advice principle:**
  * Elders’ accumulated search → reliable exploitation recommendations (e.g., best restaurants).
  * But their reduced exploration rate is interval-rational, not universally optimal for the young.
* **Well-being implication:**
  * Exploration trades present pleasure for information.
  * Algorithms (Gittins, UCB) inflate uncertain options → many disappointments.
  * Late-life focus on known favorites increases realized payoff.
  * Empirically: older adults report higher emotional satisfaction.
* **Normative life-curve:**
  * Early life: accept regret for information gain.
  * Late life: harvest accumulated knowledge.
  * Quality of life should increase as exploitation dominates.

**Key takeaways:**

* **Perceived horizon determines explore vs. exploit balance.**
* Social network pruning in aging is **strategic exploitation**, not decline.
* Experimental manipulation of time horizon shifts preferences across age groups.
* Exploration entails systematic disappointment; exploitation raises average payoff.
* Optimal lifetime policy: **explore when time is long, exploit when time is short.**

---

## Prompts

### Main Summarisation Prompt

**Role:** You are an expert in Computer Science, Operations Research, and Management Science. Your task is to analyse and summarise excerpts from a book.

**Objective:** Create a high-density summary that I can use as a quick reference for human decision-making and managerial strategy.

I will provide a section of the book, which will contain chapter/section headings denoted in Markdown format, for you to summarise. Here are what the section heading levels mean:

* ##: A chapter in the book.
* ###: A section within a chapter.

**Requirements:**

1. Technical Depth: Do not oversimplify. Retain any mathematical foundations and algorithmic logic.
2. Length Reduction: Condense the content to no more than 25% of the original length. If the provided text is very short, then condense it to no more than 50% of the original length.
3. Formatting: Markdown format, in bullet point form. Use bold for key terms. Retain the provided chapter and section headings. Do NOT add horizontal rules (divider lines).
4. Specific: Do not embellish. Summarise only the provided text.
5. Examples: Include some of the examples mentioned in the provided text to make the concepts more memorable.
6. LaTeX Usage: Use plain Unicode characters for mathematical expressions, equations, constants, etc., whenever possible, e.g., σ². In cases where they cannot be represented well with plain Unicode, use LaTeX notation, e.g., $\sum_{i=1}^{n} i$, as supported by MathJax.

At the end of your summary, write a separate "**Key takeaways:**" section heading in bold (*not* as a heading level) along with a short bullet point list of the most important concepts in the provided text. This section is for an at-a-glance of the main things to know about in the summary of the provided text. Add this section to the bottom of the summary.

After that, ask me for the next section to summarise.

Here is an example output, using lorem ipsum text as placeholder and explanatory text within parenthesis. Note the Unicode characters and Markdown notation used for mathematical expressions.

(Start of example)

## Chapter heading

### Section heading

* Lorem ipsum.
  * Dolor sit amet: σ²
* Consectetuer adipiscing $\sum_{i=1}^{n} i$ elit.

**Key takeaways:**

* **Nascetur ridiculus:** Donec quam felis.
* **Ultricies nec:** Pellentesque eu, pretium quis, sem.

Please provide the next section you would like me to summarise.

(End of example)

Ask me to provide a section to summarise now.
