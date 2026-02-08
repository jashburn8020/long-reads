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
* **Numerical illustration ($\$400\text{k}–\$500\text{k}$ range):**
  * Waiting cost = $\$1$: hold out for $\$499{,}552.79$.
  * Waiting cost = $\$2{,}000$: threshold = $\$480{,}000$.
  * Waiting cost = $\$10{,}000$: threshold = $\$455{,}279$.
  * Waiting cost $\ge \$50{,}000$ (half the range): Optimal to accept the **first offer**.
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
  * Optimal number of attempts is approximately: $\text{\# attempts} \approx \frac{p}{q}$
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
3. Formatting: Markdown format, in bullet point form. Use bold for key terms. Retain the provided chapter and section headings.
4. Specific: Do not embellish. Summarise only the provided text.
5. LaTeX Usage: Use inline or display LaTeX notation (as supported by GitHub's Markdown engine) for all mathematical formulas, constants (e.g., $1/e$), or complexity notations (where relevant info exists in the provided text.)

At the end of your summary, write a separate "**Key takeaways:**" section heading in bold (not as a heading level) along with a short bullet point list of the most important concepts in the provided text. This section is for an at-a-glance of the main things to know about in the summary of the provided text. Add this section to the bottom of the summary.

After that, ask me for the next section to summarise.

Here is an example output, using lorem ipsum text as placeholder and explanatory text within parenthesis. Note the Markdown notation used.

(Start of example)

## Chapter heading

### Section heading

* Lorem ipsum.
  * Dolor sit amet.
* Consectetuer adipiscing $1/e$ elit.

**Key takeaways:**

* **Nascetur ridiculus:** Donec quam felis.
* **Ultricies nec:** Pellentesque eu, pretium quis, sem.

Please provide the next section you would like me to summarise.

(End of example)

Ask me to provide a section to summarise now.
