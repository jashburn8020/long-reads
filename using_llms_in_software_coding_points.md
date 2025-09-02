# Using LLMs in Software Coding: Modes, Risks, and Mitigations - Points

This guide provides a mode-by-mode overview of how developers use LLMs in software engineering, what tends to go wrong, and how to mitigate long-term maintainability risks. It is intended for CTOs, engineering managers, and developers evaluating the role of LLMs in their workflow.

## 1 Coding Modes

### 1.1 "Vibe coding" (exploratory, natural-language prompts; low up-front spec)

* **Good for:** quick spikes, toy apps, UI mockups, simple CRUD, tiny CLIs.
* **Common issues:** hidden assumptions; shaky edge-case handling; silent API mismatches; code that "looks right" but fails on integration.
* **Maintainability risks:** no source of truth beyond the chat; prompt-drift (can’t reliably regenerate); inconsistent patterns/styles.
* **Mitigations:**
  * Freeze a lightweight spec (user stories + examples).
  * Convert to tests before expanding.
  * Check in the exact prompt + model/version.
  * Require a **formalized human refactor pass** with documented checklist and time budget.

### 1.2 Assistant-as-autocomplete (inline suggestions while you type)

* **Good for:** boilerplate, idiomatic snippets, test skeletons, framework glue.
* **Common issues:** confident wrong answers; outdated APIs; subtle security bugs (injection, crypto misuse).
* **Maintainability risks:** copy-paste entropy; mixed idioms; dead code paths.
* **Mitigations:**
  * Strict linters + formatters.
  * Pinned framework versions.
  * Security linters.
  * Require unit tests for suggested logic.
  * Combine developer skill with **organizational guardrails** (curated snippets, static checks, retrieval from guidelines).

### 1.3 Spec-to-scaffold (generate a service/module from a structured brief)

* **Good for:** consistent scaffolds, layer boundaries, adapters, DTOs.
* **Common issues:** brittle regeneration (overwrites human edits); spec gaps become code gaps.
* **Maintainability risks:** "regen debt" — manual edits block regeneration.
* **Mitigations:**
  * Define regen debt in docs.
  * Use templates and keep generation idempotent.
  * Isolate generated code with safe seams (protected regions, composition).

### 1.4 Code review / PR assistant

* **Good for:** style issues, obvious bugs, unreachable branches, docstrings.
* **Common issues:** shallow reasoning on architecture; missed concurrency or perf issues.
* **Maintainability risks:** false sense of safety; cargo-cult fixes.
* **Mitigations:**
  * Define reviewer tiers (style → static analysis → design).
  * Require reproducible examples for claims.

### 1.5 Bug-hunting / "why is this failing?"

* **Good for:** log triage, misconfig diffs, off-by-one, API misuse.
* **Common issues:** hallucinated framework behavior; superficial fixes that mask root cause.
* **Maintainability risks:** band-aid fixes; undocumented debugging steps.
* **Mitigations:** require minimal repro; capture root-cause narrative in PR; regression test before merge.

### 1.6 Translation & refactoring (e.g., Python→Go, class→hooks, v2→v3 API)

* **Good for:** mechanical rewrites with clear patterns.
* **Common issues:** semantic drift; missing corner features; performance regressions.
* **Maintainability risks:** partial migrations; mixed paradigms.
* **Mitigations:** characterization tests first; migrate per module; run perf tests; keep rollback plan.

### 1.7 Test generation (unit, property-based, fuzz inputs)

* **Good for:** coverage scaffolding; snapshot tests; negative cases.
* **Common issues:** assertion of implementation details; brittle snapshots.
* **Maintainability risks:** weak tests that pass but don’t protect behavior.
* **Mitigations:** focus on spec-style tests; add property/fuzz testing; review tests like production code.

### 1.8 Data/SQL & analytics coding

* **Good for:** query skeletons; join shapes; EDA scripts.
* **Common issues:** incorrect schema assumptions; subtle aggregation errors.
* **Maintainability risks:** queries that work on sample but fail in prod.
* **Mitigations:** provide schema contracts; auto-parameterize; add row-count checks.

### 1.9 Front-end UI (components, forms, state wiring)

* **Good for:** component scaffolds, accessible markup, CSS.
* **Common issues:** state bugs; fragile effects; accessibility regressions.
* **Maintainability risks:** sprawling components; inconsistent tokens.
* **Mitigations:** enforce design-system primitives; cap file complexity; generate a11y + snapshot tests.

### 1.10 Back-end endpoints & services

* **Good for:** REST/GraphQL CRUD, auth wrappers, DTO mapping.
* **Common issues:** weak auth/authorization; poor error handling.
* **Maintainability risks:** hidden coupling; missing observability.
* **Mitigations:** contract-first (OpenAPI/GraphQL); standard error envelopes; scaffold tracing and metrics.

### 1.11 Infra as Code / pipelines (Terraform, CI, Dockerfiles)

* **Good for:** boilerplate configs, CI job matrices, container recipes.
* **Common issues:** insecure defaults; over-broad IAM.
* **Maintainability risks:** snowflake CI; non-reproducible builds.
* **Mitigations:** org-approved modules; policy-as-code checks; pin versions; dry-run plans.

### 1.12 Autonomous/agentic coding (tickets→PRs end-to-end)

* **Good for:** repetitive chores (renames, config syncs).
* **Common issues:** scope drift; prompt injection; noisy commits.
* **Maintainability risks:** repo churn; regression risk.
* **Mitigations:**
  * Constrain to label-gated tasks.
  * Sandbox execution.
  * Cap PR size.
  * Require green CI + human review.

### 1.13 Documentation & examples

* **Good for:** API docs, snippets, ADR drafts.
* **Common issues:** divergence from code; invented examples.
* **Maintainability risks:** stale guidance; trust erosion.
* **Mitigations:** generate docs from annotations/contracts; run doctests; date-stamp ownership.

## 2 Cross-Cutting Themes

### 2.1 Contract-First Development (Primary Safeguard)

The single most important safeguard when coding with LLMs is **contract-first design**.

* **Why:** Contracts (API specs, schemas, invariants, property tests) form the stable "source of truth" that protects against hallucinations, hidden assumptions, and drift.
* **How:** Use OpenAPI, GraphQL, protobufs, or typed interfaces. Generate adapters and glue from contracts, while hand-crafting domain logic.
* **Result:** Enables safe regeneration, reproducibility, and long-term maintainability.

### 2.2 Determinism & Provenance

* Log model ID, hash, temperature, system prompt, and retrieval corpus snapshot. Store alongside generated artifacts.

### 2.3 Boundaries & Regeneration

* Keep generated code in distinct modules with interfaces.
* Regenerate instead of patching.

### 2.4 Style & Complexity Hygiene

* Automated linting, formatting, docstring enforcement.
* Complexity budgets for generated code.

### 2.5 Security

* Treat all LLM output as insecure by default.
* Run static security scans (injection, SSRF, crypto misuse, IAM scope).

### 2.6 Licensing & IP

* Run license scanners.
* Be mindful of copyright/data exposure in external APIs.

### 2.7 Vendor Lock-in

* Treat models like compilers.
* Pin versions, stage rollouts, and monitor regressions.

### 2.8 Observability

* Generated code must include logging, metrics, and tracing hooks.

### 2.9 Cost & Sustainability

* Cache outputs; use smaller models for routine tasks.
* Monitor API spend and environmental impact.

### 2.10 People & Skills

* Developers need prompt fluency and "LLM code review" skills.
* Prevent skill atrophy: pair juniors with mentors.
* Form an LLM working group to codify practices.

## 3 Architectural Suitability & Strategic Fit

LLM suitability depends not just on architecture, but on **contextual factors**:

* **Contract maturity** — are APIs/interfaces formally specified and testable?
* **Performance sensitivity** — does the system have strict latency/throughput requirements?
* **Security sensitivity** — would a subtle bug introduce major risk?

| Target                                 | Suitability                             | Contract maturity | Perf sensitivity | Security sensitivity | Why it works / breaks                         | Critical factors                                            |
| :------------------------------------- | :-------------------------------------- | :---------------- | :--------------- | :------------------- | :-------------------------------------------- | :---------------------------------------------------------- |
| **Scripts/CLIs/ETL**                   | **High**                                | Low–Medium        | Low              | Low                  | Small scope, clear I/O                        | Add property tests; pin deps; retries/logging               |
| **CRUD web apps**                      | **High**                                | Medium–High       | Low–Medium       | Medium               | Repetitive patterns, frameworks well-known    | Generate from schema/contracts; validate/error standards    |
| **Layered monolith (Clean/Hexagonal)** | **High**                                | High              | Medium           | Medium               | Clear seams aid safe regeneration             | Define ports/adapters; generate adapters, hand-craft domain |
| **Microservices (event-driven)**       | **Medium** (→ High if contracts mature) | High              | Medium–High      | Medium–High          | Async + idempotency are tricky                | Contract-first; simulator tests; outbox pattern             |
| **Real-time systems (trading, HFT)**   | **Low**                                 | High              | Very High        | High                 | Perf & concurrency sensitive                  | Use only for scaffolds, docs, tests                         |
| **Embedded/firmware/drivers**          | **Low**                                 | Medium            | Very High        | High                 | Hardware constraints & safety                 | Use for docs/test harnesses only                            |
| **Security-critical/cryptography**     | **Low**                                 | High              | Medium–High      | Very High            | Subtle invariants; correctness non-negotiable | Generate wrappers/tests for vetted libs; never primitives   |
| **Front-end component libraries**      | **Medium–High**                         | Medium            | Medium           | Medium               | Visual patterns easy, state is hard           | Enforce design tokens; generate tests/stories               |
| **Data pipelines/SQL models**          | **Medium**                              | Medium–High       | Medium           | Medium–High          | Schema semantics critical                     | Provide schema contracts; idempotency; totals validation    |
| **Game/graphics engines**              | **Low–Medium**                          | Low–Medium        | Very High        | Medium               | Performance & math heavy                      | Use for tools/content, not core loops                       |

## 4 Process & People: Cross-Cutting Failure Modes

This section highlights the most common and dangerous failure modes and provides quick counters. These are universal to all modes of LLM-assisted coding.

* **Hallucinated APIs/flags:** Require the model to cite versioned docs; verify with compile + typecheck in CI.
* **Shallow testing:** Ask for adversarial and boundary cases; add fuzz/property testing where applicable.
* **Concurrency bugs:** Prefer pure functions; isolate side effects; add stress tests; require timeouts/cancellation semantics.
* **Error handling holes:** Standardize error envelopes and retry/backoff; generate failure-path tests by default.
* **Cost and Resource Management:** The financial and environmental costs of using LLMs can be significant. This should be managed by using smaller models for simpler tasks, caching generations, and monitoring API usage.
* **The "LLM as a Partner" Skill Set:** Developers need to become proficient at **prompt engineering**, managing conversational context, and understanding model limitations. This new skill set is a major consideration for hiring, training, and team dynamics.
* **Prompt injection & data exfiltration:** Sanitize inputs; strip/generated prompts from untrusted code; never run suggested shell commands without review.
* **Dependency bloat/version drift:** Enforce allow-lists; run dep-pruning; pin and audit regularly.
* **Performance regressions:** Capture baseline perf tests; add budgets in CI; request complexity/perf justifications in PR templates.

## 5 Process Patterns that Make LLM Code Durable

* **Contract-first:** OpenAPI/GraphQL/Protobuf/JSON Schema as the "north star." Generate servers/clients/tests from it.
* **Example-driven:** Turn user stories into Given/When/Then examples; have the model derive tests first, then code to fit.
* **Generation boundaries:** `/generated/**` is machine-owned; hand code uses interfaces only. Regeneration is safe and reviewable.
* **Repo maps:** Feed the model a curated repo map (key files, contracts, constraints) instead of the whole tree; reduces wrong context.
* **Model as reviewer, not decider:** Bots can block on objective checks; humans approve architecture.
* **Metadata everywhere:** PRs must include the exact prompts + model/version used; CI saves them as artifacts.
* **Org-approved templates:** Maintain your own golden templates/cookbooks; don’t let the model invent structure each time.

## Prompts

The following are the most significant prompts that were used to generate the content on this page. Note that the content has been edited manually.

**ChatGPT-5:**

Explore the issues with software coding with an LLM, which can be through vibe coding, using an LLM as a coding assistant, or through any other ways. The issues I'm interested are such as the maintainability of the generated code, the types of software architecture that are suitable to be built and the code written with an LLM, and so on. Explore thoroughly by considering as many ways coding can be done with an LLM, and the associated issues with them. Provide short summaries of your exploration.

**Gemini 2.5 Flash:**

Critique the following. I am interested to know what you think about the content. I am not interested in the formatting.

(Content from the response to the prompt above.)

**Gemini 2.5 Flash:**

Reproduce the write-up that I provided to you earlier, but with your observations added to or worked into the relevant sections. I do not want you to simply append your observations to the write-up.

**Gemini 2.5 Flash:**

Review the following prompt and let me know if there are ways to improve it: "Review the write-up that you reproduced critically. Is there anything that you do not agree with or is more nuanced than the write-up suggests? Is there anything that is ambiguous or can be more clearly described? Are there considerations that have been missed or under-represented?"

**Gemini 2.5 Flash:**

You are an experienced software architect and an expert on the use of LLMs in software development. Critically analyse the write-up that you reproduced earlier. The write-up serves as a guide for developers and CTOs on the use of LLMs in software coding.

Critique the write-up based on the following criteria:

* Nuance and Disagreement: Identify any points you believe are more nuanced than the write-up suggests, or with which you disagree. For each point, explain your reasoning and provide an alternative perspective.
* Clarity and Ambiguity: Highlight any sections that are ambiguous or could be more clearly described. Suggest specific ways to improve the clarity of these sections.
* Omissions and Under-represented Considerations: Point out any important considerations that have been missed or under-represented. This could include technical, process, or architectural aspects not covered in the write-up.

Provide your analysis with clear, concise, and well-reasoned arguments. Use a professional and analytical tone throughout your response. Do not simply list points; provide a brief explanation for each.

**Gemini 2.5 Flash:**

Integrate your critique into the relevant sections in the write-up. Create new sections or rearrange the sections as necessary.

**ChatGPT-5:**

Here is a write-up of a mode-by-mode tour of how people code with LLMs—what works, what tends to go wrong, and how to keep things maintainable:

(Content from the response to the prompt above.)

You are an experienced software architect and an expert on the use of LLMs in software development. Critically analyse the write-up above. The write-up serves as a guide for developers and CTOs on the use of LLMs in software coding.

Critique the write-up based on the following criteria:

* Nuance and Disagreement: Identify any points you believe are more nuanced than the write-up suggests, or with which you disagree. For each point, explain your reasoning and provide an alternative perspective.
* Clarity and Ambiguity: Highlight any sections that are ambiguous or could be more clearly described. Suggest specific ways to improve the clarity of these sections.
* Omissions and Under-represented Considerations: Point out any important considerations that have been missed or under-represented. This could include technical, process, or architectural aspects not covered in the write-up.

Provide your analysis with clear, concise, and well-reasoned arguments. Use a professional and analytical tone throughout your response. Do not simply list points; provide a brief explanation for each.

**ChatGPT-5:**

Integrate your critique into the relevant sections in the write-up. Create new sections, rearrange the sections, update the table, etc., as necessary.
