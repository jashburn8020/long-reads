# Using LLMs in Software Coding: Modes, Risks, and Mitigations

This guide provides a mode-by-mode overview of how developers use LLMs in software engineering, what tends to go wrong, and how to mitigate long-term maintainability risks. It is intended for CTOs, engineering managers, and developers evaluating the role of LLMs in their workflow.

## 1 Coding Modes

The integration of Large Language Models (LLMs) into the software development lifecycle is not a monolithic event; rather, it manifests in a variety of distinct **coding modes**. Each mode represents a unique way a developer can interact with an LLM to assist in the creation, maintenance, or analysis of code. The effectiveness and risks of using an LLM are highly dependent on which of these modes is being employed. For a CTO, engineering manager, or developer evaluating this technology, it is crucial to understand these modes not just as a list of features, but as a framework for building a safe and efficient workflow.

This section provides a mode-by-mode overview of LLM-assisted coding, detailing the specific benefits, common issues, and long-term maintainability risks associated with each. It moves from the most informal, "vibe coding," to the more structured and strategic applications, such as spec-to-scaffold generation and autonomous agents. By examining each mode in a granular fashion, this document aims to provide a clear, actionable guide for leveraging LLMs' power while proactively mitigating the risks they can introduce to a codebase's health and durability.

### 1.1 Vibe coding

* **Good for:** quick spikes, toy apps, UI mockups, simple CRUD, tiny CLIs.
* **Common issues:** hidden assumptions; shaky edge-case handling; silent API mismatches; code that “looks right” but fails on integration.
* **Maintainability risks:** no source of truth beyond the chat; prompt-drift (can’t reliably regenerate); inconsistent patterns/styles.
* **Mitigations:**
  * Freeze a lightweight spec (user stories + examples).
  * Convert to tests before expanding.
  * Check in the exact prompt + model/version.
  * Require a **formalized human refactor pass** with documented checklist and time budget.

**"Vibe coding,"** or exploratory coding with natural language prompts, is a common entry point for developers using LLMs. It’s a fast, low-friction way to get a prototype up and running. A developer might prompt, "Create a simple Express.js server with a few endpoints for a user management system," and get a working scaffold in seconds. This mode is excellent for quick spikes, building toy applications, creating UI mockups, and generating simple CRUD (Create, Read, Update, Delete) operations or tiny command-line interfaces (CLIs). Its primary benefit is speed and the ability to rapidly iterate on an idea without getting bogged down in boilerplate code.

However, this mode is prone to a variety of issues. LLMs often make **hidden assumptions** about your environment, dependencies, or data schema, which can lead to silent failures or unexpected behavior. The generated code might "look right" but fail on integration because it handles edge cases poorly or uses slightly mismatched API versions. A common scenario is when the LLM generates code that works perfectly in isolation but breaks when connected to an existing system, perhaps because it assumes a different authentication flow or data structure.

The maintainability risks associated with vibe coding are significant. The primary issue is that there is **no single source of truth** beyond the original chat prompt, which is often lost or forgotten. This leads to **prompt-drift**, where a developer can't reliably regenerate the same code or a similar variation of it. Over time, the codebase becomes an inconsistent mix of styles and patterns, making it difficult for other team members to understand and contribute. Without a clear specification, the original intent of the code is lost, turning it into a black box that is risky to modify.

To mitigate these risks, several safeguards are essential. The first step is to **freeze a lightweight spec** before you start generating code. This doesn't need to be a formal document; it can be a simple set of user stories or a few examples that outline the desired functionality. The goal is to provide a clear, shared understanding of what the code should do. This lightweight spec becomes the source of truth, not the prompt itself.

Once the code is generated, the next crucial step is to **convert it to tests before expanding**. Treat the LLM-generated code as a draft, and immediately write unit or integration tests to confirm its functionality. This not only validates the code but also captures its intended behavior in a reproducible way. It forces a human to engage with the code, understand its logic, and formalize its contract.

To ensure long-term reproducibility, **check in the exact prompt and model version** alongside the generated code. This provides a clear provenance for the code and helps future developers understand how it was created. It's a simple practice that can save significant time and effort if the code needs to be regenerated or modified later.

Finally, and perhaps most importantly, **require a formalized human refactor pass** with a documented checklist and time budget. The generated code should never be treated as production-ready. A human developer must review it, standardize its style, document its functionality, and integrate it cleanly into the existing codebase. This refactoring phase is where the "vibe code" is transformed into durable, maintainable code. Without this critical step, the technical debt of LLM-generated code will accumulate quickly.

### 1.2 Assistant-as-autocomplete

* **Good for:** boilerplate, idiomatic snippets, test skeletons, framework glue.
* **Common issues:** confident wrong answers; outdated APIs; subtle security bugs (injection, crypto misuse).
* **Maintainability risks:** copy-paste entropy; mixed idioms; dead code paths.
* **Mitigations:**
  * Strict linters + formatters.
  * Pinned framework versions.
  * Security linters.
  * Require unit tests for suggested logic.
  * Combine developer skill with **organizational guardrails** (curated snippets, static checks, retrieval from guidelines).

The "assistant-as-autocomplete" mode is one of the most common and seamless integrations of LLMs into a developer’s workflow. Tools like GitHub Copilot or Amazon CodeWhisperer provide inline code suggestions as you type, filling in anything from a single line to an entire function. This mode is excellent for generating boilerplate code, producing idiomatic snippets for specific languages or frameworks, creating test skeletons, and writing the "glue code" that connects different parts of an application. It feels like a natural extension of a developer's own thoughts, accelerating the act of writing code and reducing repetitive typing.

Despite its convenience, this mode has a unique set of challenges. One of the most significant is the problem of **"confident wrong answers."** The LLM might provide code that looks plausible but contains subtle, non-obvious errors. It can also suggest **outdated APIs** or patterns that have been deprecated, introducing technical debt from the very beginning. Developers can unknowingly accept these suggestions, thinking they are correct, only to discover issues much later in the development cycle. Even more concerning are **subtle security bugs**, such as injection vulnerabilities or the misuse of cryptographic functions, which are easy to overlook in an inline suggestion.

The maintainability risks of this approach are also substantial. Constant acceptance of inline suggestions can lead to **"copy-paste entropy,"** where different parts of the codebase, even those performing similar tasks, end up with slightly different implementations. This introduces inconsistencies and makes the code harder to reason about. It can also result in a codebase with a **mix of idioms**, as the LLM might switch between different best practices or styles, making it difficult to establish a single, unified coding standard. Furthermore, these suggestions can sometimes create **dead code paths**—code that is never executed—which clutters the codebase and increases the surface area for bugs.

To mitigate these issues, a multi-layered approach is necessary. First, **strict linters and formatters** are non-negotiable. These tools can automatically catch many stylistic and structural inconsistencies introduced by the assistant. Pinned framework versions in your project's configuration are also crucial; this helps ensure the LLM suggestions align with the specific APIs you're using, rather than a generic or outdated version. For security, **security linters** or static application security testing (SAST) tools should be integrated into the workflow to automatically flag potential vulnerabilities in the suggested code.

Beyond automated checks, it's vital to create a process that requires a human to validate the code. A good practice is to **require unit tests for any non-trivial logic** generated by the assistant. This forces the developer to manually review the code, understand its behavior, and prove its correctness.

Finally, and most importantly, this mode works best when a developer's skill is combined with **organizational guardrails.** Companies should invest in creating and maintaining curated snippets and guidelines that LLMs can draw from. For example, using a tool that can retrieve from your internal code knowledge base helps ensure that suggestions are not just syntactically correct but also follow your organization's established patterns and security protocols. This transforms the assistant from a general-purpose tool into a tailored, on-brand coding partner.

### 1.3 Spec-to-scaffold

* **Good for:** consistent scaffolds, layer boundaries, adapters, DTOs.
* **Common issues:** brittle regeneration (overwrites human edits); spec gaps become code gaps.
* **Maintainability risks:** “regen debt” — manual edits block regeneration.
* **Mitigations:**
  * Define regen debt in docs.
  * Use templates and keep generation idempotent.
  * Isolate generated code with safe seams (protected regions, composition).

Spec-to-scaffold is a more structured and robust mode of using LLMs, where the developer provides a structured brief or a formal specification to generate a foundational codebase. This approach moves beyond simple natural language prompts to leverage a more defined input, such as a JSON schema, an OpenAPI specification, or a detailed set of requirements. This method is particularly **good for** creating consistent scaffolds for new services, defining clear layer boundaries in a microservices architecture, generating adapters for external APIs, or creating data transfer objects (DTOs) and API contracts. It ensures that the generated code adheres to a predefined structure, which is a major advantage over the ad-hoc nature of "vibe coding."

However, this mode is not without its challenges. A primary issue is **brittle regeneration**, where subsequent generations of the scaffold can overwrite human edits. Developers often need to manually modify the generated code to add business logic or fine-tune its behavior. If the spec changes and the scaffold needs to be regenerated, these manual edits can be lost, creating a frustrating and error-prone workflow. Another common problem is that **spec gaps become code gaps**; if the initial specification is incomplete or ambiguous, the generated code will also be incomplete or contain logical flaws, requiring extensive manual intervention to fix.

The key **maintainability risk** here is what's known as **"regen debt."** This debt accumulates when manual edits block the ability to regenerate the code from its source spec. The more a developer modifies the generated code, the more difficult it becomes to update it automatically when the underlying spec changes. This can lead to a situation where the team either stops using the generation tool altogether or spends a disproportionate amount of time manually porting changes, negating the initial time savings.

To mitigate these risks, several strategies should be employed. First, it is crucial to **define "regen debt" in the documentation** for the project. Making the team aware of this concept and its implications helps them prioritize preserving the ability to regenerate code. This means developers must be thoughtful about how and where they add their custom logic.

Next, focus on making the generation process **idempotent** by using templates and careful design. This means running the generation process multiple times with the same input should produce the same output without unintended side effects. This is often achieved by generating code into a distinct directory or file, rather than overwriting existing files that might contain human edits.

Most critically, the generated code must be **isolated with safe seams.** This is a core architectural pattern where the generated code is treated as a component with well-defined entry and exit points. Developers should be instructed to add their business logic and customizations **outside** of the generated files. This can be done using **protected regions** (code blocks that are marked with special comments that tell the generator not to touch them), or more robustly, by using **composition** and dependency injection. For example, the generated scaffold might contain a base class or an interface, and the human-written code would then extend or implement it, keeping the two separate. This approach ensures that the generated code can be regenerated at any time without fear of losing critical human contributions.

### 1.4 Code review / PR assistant

* **Good for:** style issues, obvious bugs, unreachable branches, docstrings.
* **Common issues:** shallow reasoning on architecture; missed concurrency or perf issues.
* **Maintainability risks:** false sense of safety; cargo-cult fixes.
* **Mitigations:**
  * Define reviewer tiers (style → static analysis → design).
  * Require reproducible examples for claims.

The use of an LLM as a code review or Pull Request (PR) assistant is a powerful application that can significantly streamline the development process. In this mode, the LLM analyzes a new code submission and provides feedback directly to the developer or the reviewer. This is particularly **good for** catching simple **style issues**, identifying **obvious bugs** or syntax errors, pointing out **unreachable code branches**, and generating or improving **docstrings** and comments. It can quickly handle the low-level, mechanical aspects of a review, freeing up human reviewers to focus on higher-order concerns. The assistant acts as a tireless first-pass reviewer, ensuring code quality standards are met before a human even looks at the changes.

However, a key limitation of the LLM as a review assistant is its **shallow reasoning on architecture**. While it can see and comment on the code in front of it, it often lacks the broader context of the entire system. This means it can miss issues related to how the new code integrates with other services, how it affects a microservice contract, or whether it aligns with the system's long-term architectural vision. It can also easily miss more complex issues, such as **concurrency problems** or subtle **performance bottlenecks**, which require a deep understanding of the system's state and runtime behavior.

The main **maintainability risks** here stem from a **false sense of safety**. A developer might see the LLM's "thumbs up" on a PR and assume the code is robust and correct, leading them to merge it without sufficient human scrutiny. This can lead to a phenomenon called **"cargo-cult fixes,"** where the LLM suggests a change that happens to fix one specific issue but doesn't address the underlying problem or introduces new, hidden bugs. The developer, trusting the assistant, applies the fix without fully understanding why it works, leading to a brittle and poorly understood codebase.

To mitigate these risks, it's essential to establish clear guidelines for how the LLM assistant fits into the review process. First, **define reviewer tiers**. The LLM should be considered a "tier 1" reviewer, responsible for style and static analysis checks. Human reviewers, in contrast, should be elevated to "tier 2" and "tier 3" roles, focusing on design patterns, architectural fit, and business logic validation. The LLM's feedback should be seen as a recommendation, not a final verdict.

Additionally, to ensure the assistant's feedback is grounded in reality, you should **require reproducible examples for claims**. If the LLM suggests a bug exists, the developer should be prompted to write a small test case that demonstrates the issue. This forces the human to engage critically with the LLM's output and validates its suggestions, transforming the assistant from a black box into a tool for collaborative debugging. This process ensures that the code that is ultimately merged is both stylistically sound and functionally correct, with a clear understanding of its behavior.

### 1.5 Bug-hunting / "why is this failing?"

* **Good for:** log triage, misconfig diffs, off-by-one, API misuse.
* **Common issues:** hallucinated framework behavior; superficial fixes that mask root cause.
* **Maintainability risks:** band-aid fixes; undocumented debugging steps.
* **Mitigations:** require minimal repro; capture root-cause narrative in PR; regression test before merge.

Using an LLM for bug-hunting is like having an extra pair of eyes that can quickly scan logs, code, and configurations to pinpoint potential problems. This mode is excellent for **log triage**, helping a developer make sense of a large, complex stack trace and identifying the most likely culprit. It can also be very effective at spotting simple but elusive issues like **misconfiguration diffs** (where a tiny change in a configuration file breaks something), **off-by-one errors** in loops or arrays, and instances of **API misuse**, where a developer has called a function with incorrect parameters. The LLM can digest vast amounts of information in a short time, making it a powerful diagnostic aid.

A significant issue, however, is the LLM's tendency to offer a fix that is a **superficial patch rather than a solution to the root cause.** For example, it might suggest adding a `try-catch` block to suppress an error, when the real problem is a logical flaw in the code. A more dangerous problem is when the LLM **hallucinates framework behavior**, confidently explaining why a bug exists based on a nonexistent feature or a misunderstanding of how a library works. This can send a developer down a rabbit hole, wasting time and introducing even more complexity.

The primary **maintainability risks** associated with this mode are the accumulation of **band-aid fixes** and **undocumented debugging steps**. If a fix is just a quick patch, it doesn't solve the underlying problem, and the bug is likely to reappear in a different form later. Furthermore, the valuable context and insights gained during the debugging process, which are often shared in a chat with the LLM, are rarely captured and documented. This means the next developer who encounters a similar issue has to start the entire debugging process from scratch.

To mitigate these risks, a few practices are essential. First, **require a minimal reproducible example** (a "minimal repro") before accepting a fix. This forces the developer to isolate the bug from the rest of the application, ensuring they truly understand the problem. The LLM can be a valuable partner in creating this minimal repro, but the human must validate it.

Second, the solution's narrative must be captured. **Capture the root-cause narrative in the PR or commit message.** This means explaining not just what was changed, but why the change was necessary and what the original problem was. This creates a valuable knowledge base for the team.

Finally, a **regression test must be added before the merge.** This is a non-negotiable step that formalizes the bug fix. The test should be designed to fail with the old, buggy code and pass with the new, fixed code. This prevents the bug from ever re-entering the codebase and ensures the fix is durable.

### 1.6 Translation & refactoring

* **Good for:** mechanical rewrites with clear patterns.
* **Common issues:** semantic drift; missing corner features; performance regressions.
* **Maintainability risks:** partial migrations; mixed paradigms.
* **Mitigations:** characterization tests first; migrate per module; run perf tests; keep rollback plan.

Using an LLM for code translation and refactoring is a compelling use case because it automates what is often a tedious and error-prone task. This mode is particularly **good for** **mechanical rewrites with clear patterns**, such as migrating code from one language to another (e.g., Python to Go), updating framework patterns (e.g., React class components to hooks), or adapting to a new API version (e.g., a v2 to v3 API). The LLM can quickly apply known transformations across a large codebase, saving immense developer time and ensuring a consistent result.

However, the process is far from perfect. A major issue is **semantic drift**, where the translated code looks correct but subtly changes the original behavior. This is especially true for languages with different idiomatic patterns or type systems. The LLM might also **miss corner features** or edge cases that are not well-represented in its training data, leading to incomplete or buggy translations. A critical risk is **performance regressions**, as a direct, literal translation might not leverage the new language's or framework's performance optimizations, resulting in a slower application.

The primary **maintainability risks** lie in **partial migrations** and **mixed paradigms**. If only part of a codebase is translated, you can end up with a hybrid system that is harder to understand and maintain. The two codebases might use different data structures, error handling, or concurrency models, making them difficult to integrate and reason about. This "mixed paradigm" creates a long-term maintenance headache and a new class of bugs at the boundaries between the old and new code.

To mitigate these risks, a few key strategies are essential. First, before any translation begins, **characterization tests must be written**. These tests don't just check for functionality; they **characterize** the existing code's behavior, including its edge cases and side effects. They serve as the golden standard for the translated code, ensuring that the behavior remains unchanged.

Next, a migration plan should be in place. It's often best to **migrate per module or per service** rather than attempting a large, monolithic translation. This allows for a more controlled rollout, with each piece being independently verified before moving on. It also helps to prevent a "mixed paradigm" codebase by keeping the two versions of the code distinct until a module is fully migrated.

Finally, you must **run performance tests** on the new code to ensure it's not a step backward. This is a critical check for ensuring the new code is not just functionally correct but also efficient. And, as with any major change, you must **keep a rollback plan** ready, should the translated code prove to be unstable or problematic. This provides a safety net and reduces the risk of committing to a broken migration.

### 1.7 Test generation

* **Good for:** coverage scaffolding; snapshot tests; negative cases.
* **Common issues:** assertion of implementation details; brittle snapshots.
* **Maintainability risks:** weak tests that pass but don’t protect behavior.
* **Mitigations:** focus on spec-style tests; add property/fuzz testing; review tests like production code.

Using an LLM to generate tests is a highly effective way to accelerate a crucial but often neglected part of the software development lifecycle. This mode is **good for** creating **coverage scaffolding**—quickly generating a basic set of unit tests for a new function or module to ensure some level of coverage from the start. It also excels at creating **snapshot tests** for UI components and generating **negative cases** to test error handling and unexpected inputs. The LLM can quickly produce a large volume of test code, freeing up a developer to focus on more complex test scenarios.

However, the quality of these tests can be a significant issue. LLMs often produce tests that **assert implementation details** rather than the behavior of the code. For example, a test might check if a specific private method was called or if an internal data structure has a certain value, which makes the test brittle. If the implementation changes, the test will fail, even if the public behavior of the code remains the same. This leads to **brittle snapshots**, where a small, non-breaking change to a component's rendering causes a large number of snapshot tests to fail, requiring a tedious update process.

The main **maintainability risk** is that the generated tests are **weak and don’t protect behavior**. They might provide a false sense of security, passing on every build but failing to catch real-world bugs. This weakens the entire testing suite, making it a liability rather than a safeguard. Developers might stop trusting the tests, leading to a decline in testing discipline.

To mitigate these risks, a developer must be prescriptive about the type of tests they want. Instead of asking for a generic unit test, a developer should **focus on spec-style tests**. These tests are written to a clear specification and assert the public-facing behavior of the code, not its internal workings. For example, a test should check if a function returns the correct output for a given input, not how it arrived at that output.

Furthermore, developers should **add property/fuzz testing**. These are more powerful forms of testing where the LLM can generate a wide range of random or structured inputs to find edge cases that a human might miss. This moves beyond simple examples and ensures the code is robust against unexpected data.

Finally, the generated tests must be treated as first-class citizens. You must **review tests like production code**. This means they should be subject to the same rigorous code review process, checking for clarity, robustness, and adherence to team standards. The goal is to ensure that the tests are not just passing but are also a reliable and durable part of the codebase.

### 1.8 Data/SQL & analytics coding

* **Good for:** query skeletons; join shapes; EDA scripts.
* **Common issues:** incorrect schema assumptions; subtle aggregation errors.
* **Maintainability risks:** queries that work on sample but fail in prod.
* **Mitigations:** provide schema contracts; auto-parameterize; add row-count checks.

Using an LLM for data and SQL coding is a powerful way to accelerate the work of data analysts and backend engineers. This mode is **good for** generating **query skeletons** (e.g., `SELECT ... FROM ... WHERE ...`), defining complex **join shapes**, and writing **exploratory data analysis (EDA) scripts**. Instead of manually writing boilerplate SQL, a developer can use a natural language prompt like, "Show me the average revenue per customer for the last quarter, grouped by region," to get a working query. This saves significant time and lowers the barrier to entry for team members who might be less familiar with SQL syntax.

A significant issue, however, is the LLM's tendency to make **incorrect schema assumptions**. If the LLM doesn't have access to the exact database schema, it might guess column names or table relationships, leading to syntax errors or, worse, queries that run but return no results. Another common problem is **subtle aggregation errors**, where the LLM might use an incorrect aggregate function or misapply a grouping, leading to seemingly correct but factually wrong insights. These errors can be very difficult to spot because the queries appear to be syntactically valid.

The primary **maintainability risk** is that the generated **queries work on sample data but fail in production**. A query might work perfectly on a small, curated dataset but break when faced with the nuances of production data, such as `NULL` values, different data types, or a much larger volume of data. This creates a brittle analytics pipeline that is prone to unexpected failures.

To mitigate these risks, the LLM needs more context and guardrails. First, you must **provide schema contracts** to the LLM. This can be done by including the `CREATE TABLE` statements or a simple list of tables and columns in the prompt. This ensures the LLM generates queries that are syntactically correct and align with your database structure.

Next, a critical practice is to **auto-parameterize** the generated queries. Instead of hardcoding values, the LLM should be instructed to use placeholders (e.g., `WHERE customer_id = ?`) which are then filled by the application. This prevents SQL injection vulnerabilities and makes the queries more reusable.

Finally, to prevent data integrity issues, you must **add row-count checks**. A good practice is to include a step in the process that compares the number of rows returned by the LLM-generated query with a known baseline or an expected range. This can quickly flag queries that are not returning the expected results. This, combined with human review and validation against source-of-truth metrics, ensures that the generated analytics are both correct and reliable.

### 1.9 Front-end UI

* **Good for:** component scaffolds, accessible markup, CSS.
* **Common issues:** state bugs; fragile effects; accessibility regressions.
* **Maintainability risks:** sprawling components; inconsistent tokens.
* **Mitigations:** enforce design-system primitives; cap file complexity; generate a11y + snapshot tests.

Using LLMs for front-end development, from component creation to state management, is becoming an increasingly popular use case. This mode is particularly **good for** generating **component scaffolds**, creating a basic structure for a button, a form, or a navigation bar based on a description. It also excels at writing **accessible markup** by correctly applying ARIA attributes and semantic HTML, and it's a great tool for generating complex **CSS** or CSS-in-JS that adheres to a design system. This automates the repetitive parts of front-end development, letting developers focus on the user experience and business logic.

A primary issue is that LLMs often introduce subtle **state bugs** or **fragile effects**. The LLM might not correctly manage a component's internal state or could write side effects that work in isolation but break when a user interacts with the component in a complex way. This can lead to bugs that are difficult to reproduce and debug. Furthermore, a reliance on LLMs can lead to **accessibility regressions**, as the model might produce code that passes basic checks but fails to account for more nuanced accessibility requirements.

The key **maintainability risks** lie in the creation of **sprawling components** and **inconsistent tokens**. An LLM might generate a single, monolithic component that handles too many responsibilities, making it hard to read and refactor. This goes against the best practice of creating small, single-purpose components. Additionally, without proper guardrails, the LLM can generate CSS or styling that uses inconsistent tokens (e.g., using `blue-500` in one place and `#0000ff` in another for the same color), leading to a chaotic and unmaintainable stylesheet.

To mitigate these risks, you should start by **enforcing design-system primitives**. Instead of asking the LLM to generate raw HTML and CSS, instruct it to use your pre-defined component library and design tokens. This ensures that the generated code is consistent and on-brand from the start. A good practice is to provide a brief to the LLM that includes a list of available components and their properties.

It's also essential to **cap file complexity**. Establish a maximum line count or cyclomatic complexity for files, and use automated checks to enforce this rule. This forces developers to refactor large, auto-generated components into smaller, more manageable units.

Finally, you must use automation to validate the output. **Generate accessibility and snapshot tests** as part of the initial prompt. The LLM can create a basic set of tests to validate the component's output, and a human can then refine them to ensure they cover all the critical user interactions. The **accessibility tests** ensure the generated code adheres to WCAG guidelines, and **snapshot tests** provide a safety net against unintended UI changes.

### 1.10 Back-end endpoints & services

* **Good for:** REST/GraphQL CRUD, auth wrappers, DTO mapping.
* **Common issues:** weak auth/authorization; poor error handling.
* **Maintainability risks:** hidden coupling; missing observability.
* **Mitigations:** contract-first (OpenAPI/GraphQL); standard error envelopes; scaffold tracing and metrics.

Using an LLM to generate back-end code is an effective way to quickly create the infrastructure needed for an application's data layer and business logic. This mode is **good for** generating boilerplate for common tasks like **REST/GraphQL CRUD** (Create, Read, Update, Delete) operations, creating **authentication wrappers**, and mapping between different data structures (e.g., **DTO mapping**). A developer can prompt for a new endpoint to handle user sign-ups, and the LLM can generate the route, the controller logic, and the database interaction code. This accelerates the process of standing up a new service and ensures a consistent pattern for common tasks.

A major risk, however, is the LLM's tendency to generate code with **weak authentication and authorization** logic. While the code may handle the happy path, it can easily miss crucial security checks, leaving endpoints vulnerable to unauthorized access. Another common issue is **poor error handling**, where the LLM might generate code that simply returns a generic 500 server error, obscuring the underlying problem and making debugging difficult. This lack of robust error handling can also expose sensitive information in stack traces, posing a security risk.

The key **maintainability risks** of this mode are **hidden coupling** and **missing observability**. The LLM might generate code that tightly couples different components or services in a way that is not immediately obvious, making future changes difficult. This can lead to a brittle architecture that breaks unexpectedly when one part of the system is modified. Furthermore, the generated code often lacks crucial **observability** features such as logging, tracing, and metrics, making it a black box that is impossible to monitor or debug in a production environment.

To mitigate these risks, a **contract-first** approach is essential. Before any code is generated, a formal contract—such as an **OpenAPI or GraphQL schema**—should be defined. This contract serves as the source of truth for the API and ensures that the generated code adheres to a well-defined interface. The LLM's role is then to implement this contract, not to define it.

Next, you must establish and enforce standards for error handling. A good practice is to require the LLM to use **standard error envelopes** that return consistent, well-structured error messages. This makes the API more predictable and easier for front-end clients to consume.

Finally, to address the lack of observability, the LLM should be prompted to **scaffold tracing and metrics** alongside the business logic. This means generating code that includes hooks for a distributed tracing system (e.g., OpenTelemetry) and metrics libraries. This ensures the service is observable from day one, providing a crucial safety net for production environments.

### 1.11 Infra as Code / pipelines

* **Good for:** boilerplate configs, CI job matrices, container recipes.
* **Common issues:** insecure defaults; over-broad IAM.
* **Maintainability risks:** snowflake CI; non-reproducible builds.
* **Mitigations:** org-approved modules; policy-as-code checks; pin versions; dry-run plans.

Using an LLM for Infrastructure as Code (IaC) and pipeline automation is a powerful way to standardize and accelerate the provisioning of infrastructure. This mode is **good for** generating **boilerplate configurations** for tools like Terraform, defining complex **CI job matrices** in a YAML file, or creating robust **container recipes** in Dockerfiles. A developer can describe the desired infrastructure in natural language—"create a Terraform module for a simple S3 bucket with public read access"—and get a working, and potentially reusable, snippet. This saves significant time and helps ensure consistency across different projects.

However, this is also a high-stakes domain. A major issue is that LLMs can generate **insecure defaults**. For example, a generated Terraform script for a storage bucket might default to public access, a common misconfiguration that leads to data leaks. Another serious problem is the creation of **over-broad IAM (Identity and Access Management)** policies, which grant permissions far beyond what's needed, creating a significant security vulnerability. Because IaC directly manages production resources, even small errors can have large consequences.

The primary **maintainability risks** are **"snowflake CI"** environments and **non-reproducible builds**. If LLM-generated pipeline code is not standardized, each CI/CD pipeline becomes a unique "snowflake" that is difficult to manage, debug, and update. Over time, these pipelines become a collection of manual patches and undocumented workarounds. Furthermore, a non-reproducible build can be a nightmare to debug and audit, making it impossible to guarantee that the same code will produce the same production artifact.

To mitigate these risks, strict guardrails are non-negotiable. First, you must mandate the use of **organization-approved modules**. Instead of allowing the LLM to generate raw IaC code, it should be instructed to use pre-vetted, security-hardened modules from an internal registry. This ensures that the generated code is built on a secure foundation.

Next, you must implement a robust **policy-as-code (PaC) framework**. Tools like Open Policy Agent (OPA) can automatically check the generated IaC for security and compliance violations before it's ever applied. This acts as an automated security review, catching insecure configurations before they reach production.

Finally, you must **pin all versions** of containers, runners, and dependencies in your CI/CD pipelines to ensure reproducibility. And before any changes are applied, you must **dry-run the plan**. For Terraform, this means running `terraform plan` to see exactly what changes will be made to the infrastructure without applying them. This final human review and automated check provide a critical safety net against unintended consequences.

### 1.12 Autonomous/agentic coding

* **Good for:** repetitive chores (renames, config syncs).
* **Common issues:** scope drift; prompt injection; noisy commits.
* **Maintainability risks:** repo churn; regression risk.
* **Mitigations:**
  * Constrain to label-gated tasks.
  * Sandbox execution.
  * Cap PR size.
  * Require green CI + human review.

The ultimate vision for LLM-assisted coding is the **autonomous agent**—a system that can take a task, from a bug report or a feature ticket, and independently generate, test, and submit a pull request (PR). This mode is exceptionally **good for** automating repetitive, low-complexity chores like a file-wide renaming operation or a configuration synchronization across multiple repositories. It promises to free up developer time for more creative and complex problem-solving. An agent can be instructed to "add a new field to the user profile API, including the database migration and documentation," and it would, in theory, handle the entire process without human intervention.

However, this mode is laden with risks. A primary issue is **scope drift**, where the agent, in its attempt to solve a problem, makes changes far beyond the original request, introducing unintended side effects or technical debt. For instance, an agent tasked with a small refactor might decide to update a library, which then triggers a cascade of other changes. Another critical vulnerability is **prompt injection**, where a malicious actor could embed a hidden command in a ticket description, causing the agent to perform an unauthorized or destructive action. The agent can also create **noisy commits** and PRs, with verbose, machine-generated messages that lack the human context and narrative crucial for a good version control history.

The **maintainability risks** are profound. Unchecked agent activity can lead to significant **repository churn**, where the codebase is constantly in flux, making it difficult for humans to follow the evolution of the project. This also introduces a high **regression risk**, as a small, seemingly harmless agent-generated change can break a critical part of the system in a way that is difficult to trace back to its source. The lack of human oversight in the creation of the code means the team has little to no understanding of why or how the changes were made.

To mitigate these risks, the agent must operate within a well-defined and constrained environment. First, **constrain the agent to label-gated tasks.** The agent should only be allowed to work on tickets with a specific label, like `agent-safe` or `chore`, that explicitly grants it permission to operate. This provides a clear human-in-the-loop control.

Next, you must **sandbox the execution**. The agent's environment should be isolated from production systems and have limited permissions. This prevents a misbehaving or malicious agent from causing damage to live infrastructure or data.

Third, **cap the PR size.** An agent should be configured to submit small, single-purpose pull requests. This makes the human review process manageable and reduces the risk of a massive, hard-to-review change being merged.

Finally, you must **require a green CI build and human review** for every single PR submitted by the agent. The green CI build acts as a final sanity check, ensuring the changes don't break any existing tests. The human review, no matter how brief, provides a critical checkpoint where a developer can verify the changes, add their own context, and approve the merge, ensuring the code is not only functional but also understandable and maintainable. This turns the agent from a fully autonomous system into a powerful, but safely constrained, coding partner.

### 1.13 Documentation & examples

* **Good for:** API docs, snippets, ADR drafts.
* **Common issues:** divergence from code; invented examples.
* **Maintainability risks:** stale guidance; trust erosion.
* **Mitigations:** generate docs from annotations/contracts; run doctests; date-stamp ownership.

Using an LLM for documentation is one of its most valuable and least risky applications. This mode is particularly **good for** generating **API docs** from code comments, creating simple **code snippets** for a tutorial, and drafting **ADR (Architectural Decision Records)**. A developer can prompt for a detailed explanation of a function's purpose and its parameters, and the LLM can quickly produce a well-structured docstring. This accelerates the often-tedious process of writing and maintaining documentation, ensuring a higher level of coverage.

A primary issue is the **divergence from code**. If the documentation is not directly tied to the codebase, the LLM can generate documentation that quickly becomes stale as the code evolves. This leads to an inaccurate and misleading reference. A related problem is that the LLM can create **invented examples** that don't actually work or are inconsistent with the documented behavior. These examples can mislead developers and cause confusion.

The key **maintainability risks** are **stale guidance** and **trust erosion**. When documentation is inaccurate, it becomes a liability rather than an asset. Developers stop trusting it and resort to reading the code itself, negating the purpose of the documentation. This erosion of trust in the documentation can lead to a long-term decline in its quality and utility.

To mitigate these risks, the documentation process must be automated and tied to the code itself. First, you should **generate docs from annotations/contracts**. Tools that can parse code comments, OpenAPI specs, or other structured contracts should be used to create the documentation. This ensures that the documentation is always a direct reflection of the code's current state.

Next, you must **run doctests**. These are automated tests that execute the code examples within the documentation to ensure they are correct. This prevents the problem of invented or broken examples, guaranteeing that the snippets in the docs actually work.

Finally, you should **date-stamp ownership**. Each documentation page or section should have a clear date of last update and an owner. This adds a layer of accountability, making it easy to identify stale content and assign someone to review and update it. By treating documentation with the same rigor as production code, you can ensure it remains a valuable and trustworthy resource for the entire team.

## 2 Cross-Cutting Themes

While each LLM-assisted coding mode presents unique benefits and risks, a set of universal mitigations applies to all of them. These **cross-cutting themes** are not specific to a single use case but are foundational principles that, when implemented, create a durable and secure LLM-assisted software development practice. They act as a strategic framework, ensuring that the code generated is not just a quick fix but a long-term, maintainable asset. By focusing on these principles—ranging from architectural patterns to process and people-oriented practices—organizations can safely and effectively integrate LLMs into their workflow, transforming a powerful tool into a reliable partner.

### 2.1 Contract-First Development (Primary Safeguard)

The single most important safeguard when coding with LLMs is **contract-first design**.

* **Why:** Contracts (API specs, schemas, invariants, property tests) form the stable “source of truth” that protects against hallucinations, hidden assumptions, and drift.
* **How:** Use OpenAPI, GraphQL, protobufs, or typed interfaces. Generate adapters and glue from contracts, while hand-crafting domain logic.
* **Result:** Enables safe regeneration, reproducibility, and long-term maintainability.

The single most important safeguard when using LLMs in software development is **contract-first development**. This approach establishes a stable, explicit agreement or **contract** before any code is written, serving as the ultimate source of truth. This is a primary safeguard because it protects against the most common pitfalls of LLM-generated code: hallucinations, hidden assumptions, and prompt-drift. Instead of relying on a vague natural language prompt, the LLM is guided by a formal, machine-readable specification. The contract acts as a shared blueprint for the system's behavior, ensuring consistency across all generated components.

The "why" behind this approach is simple: **contracts protect against hallucinations**. LLMs are prone to making up APIs, functions, or data formats that don't exist. By providing a formal contract—such as a well-defined API schema—you constrain the LLM's output to a valid and predictable structure. The contract also exposes **hidden assumptions** that a developer might not even realize they have. For example, a contract forces you to define data types, validation rules, and error states upfront, which the LLM can then correctly implement.

The "how" involves using a variety of formalisms. For APIs, this means defining the interface using a tool like **OpenAPI**, **GraphQL**, or **protobufs** before generating any code. For internal components, it could mean defining **typed interfaces** in languages like TypeScript or Go, or defining data **schemas** with tools like JSON Schema. The key is to generate the adapters and "glue code" from these contracts, while leaving the complex, business-specific domain logic to be hand-crafted by a human developer. This creates a clear separation of concerns, ensuring that the critical parts of the system are not left to an LLM.

This process has a clear and powerful result: it enables **safe regeneration** and **reproducibility**. Because the contract is the source of truth, you can regenerate the scaffold or boilerplate at any time without fear of overwriting human-written code. As long as the contract remains stable, the generated code will also remain consistent. This also contributes to long-term **maintainability**, as new team members can easily understand the system's behavior by simply reading its contracts, which are far more reliable than a scattered collection of natural language prompts.

Contract-first development also has a positive impact on the overall architecture. It naturally leads to systems with clear boundaries and well-defined interfaces, which are easier to test, debug, and scale. It forces the team to think about the system's external behavior before its internal implementation.

In essence, the contract acts as a shared mental model that the LLM and the human developer can both work from. It elevates the conversation from "what code should I write?" to "what should this system do?" This strategic shift is fundamental to building durable, reliable software with LLM assistance.

### 2.2 Determinism & Provenance

* Log model ID, hash, temperature, system prompt, and retrieval corpus snapshot. Store alongside generated artifacts.

When using LLMs for coding, ensuring **determinism and provenance** is critical for long-term maintainability. Determinism means that the same input (prompt, context, and model) will consistently produce the same output. Provenance refers to the ability to trace the origin of a piece of generated code. Without these, you create code that is a black box—you know it works now, but you can't guarantee it will be reproducible in the future, nor can you easily understand how it was created. This lack of traceability makes debugging and security audits nearly impossible.

The core of this practice is to **log all relevant metadata** alongside the generated code. This includes a snapshot of the **model ID** and **version**, the exact **system prompt** and **user prompt** that were used, and the **temperature** setting. It's also vital to capture a snapshot of the **retrieval corpus**—the specific documents, code snippets, or internal knowledge base the LLM drew from. This acts as a timestamped "recipe" for the generated artifact. By storing this information, you create a clear audit trail that can be used to debug issues and to reproduce the code if needed.

To make this practical, you should **store this metadata as a standard artifact**. This could be a JSON file or a structured comment block embedded in the generated code itself. This ensures the provenance information is always tied to the code it describes. For instance, a generated file might begin with a comment block detailing the prompt, the model used, and the timestamp. This simple practice ensures that future developers can look at a piece of code and understand exactly how it came into existence.

The importance of this goes beyond just reproducibility. Provenance is a key part of **security and compliance**. In a security audit, you can be asked to justify why a specific piece of code was added. With a clear provenance record, you can easily trace the code back to its original prompt, demonstrating that it was generated for a specific, authorized purpose. This is a non-negotiable step for organizations in regulated industries.

Finally, a commitment to determinism and provenance forces a discipline of **structured prompting**. Instead of ad-hoc, one-off prompts, developers are encouraged to use and reuse standardized prompts. This not only improves the quality of the generated code but also makes it easier to track and manage. The goal is to move from a chat-based, "vibe coding" approach to a more industrial, repeatable process.

### 2.3 Boundaries & Regeneration

* Keep generated code in distinct modules with interfaces.
* Regenerate instead of patching.

A critical cross-cutting mitigation for using LLMs in software development is the establishment of clear **boundaries and regeneration strategies**. This principle dictates that LLM-generated code should be treated differently from human-written code. The main goal is to prevent the common pitfall of "regeneration debt," which occurs when a developer makes manual edits to a generated file, effectively "locking" it from future, automated updates. By separating concerns and defining clear boundaries, you ensure that the benefits of LLM-assisted coding—speed and consistency—remain a long-term asset, not a one-time gain.

The most effective way to implement this is to **keep generated code in distinct modules with interfaces**. Instead of allowing an LLM to inject code directly into an existing file, you should configure your workflow to output the generated artifacts into a specific directory or module. This generated code should expose its functionality through a well-defined public interface, while keeping its internal implementation hidden. A human developer's code can then interact with this generated module through its stable interface, creating a "safe seam" between the two. This pattern is similar to a microservice boundary, where the consumer is shielded from the provider's internal changes as long as the API contract remains the same.

A key part of this approach is the cultural and process-based shift to **regenerate instead of patching**. When a change is required, the first instinct should be to update the original prompt, spec, or template and regenerate the code, rather than manually editing the generated output. This practice reinforces the source of truth—the prompt or spec—and ensures that the codebase remains consistent with its defined contracts. It also prevents the accumulation of undocumented, ad-hoc fixes that are hard to maintain. The process should be simple enough that regenerating is easier than making a manual, one-off change.

To further support this, you should make the code generation process **idempotent**. This means running the generation command multiple times with the same inputs should always produce the same, correct output without side effects. Tools and scripts that support this are crucial. For example, a generator could be configured to create a new file with the suffix `.generated.ts` and merge its contents, or use special comment blocks like `// BEGIN GENERATED CODE` and `// END GENERATED CODE` to create protected regions within an existing file. This gives developers a way to add their own custom logic while still allowing the LLM-generated parts to be updated automatically.

This strategy also has a significant impact on team collaboration. When a new developer joins a project, they can quickly understand which parts of the codebase are generated and which are hand-written. This clarity reduces the cognitive load and helps them avoid making changes that would break the regeneration process. It also makes it easier to onboard new team members to the LLM-assisted workflow, as the rules for interaction are explicit and well-defined.

Ultimately, a well-defined boundary strategy turns the LLM from a simple code generator into a powerful automation tool. It shifts the developer's mindset from fixing the output to refining the input (the prompt or spec). This is a strategic move from low-level, tactical fixes to high-level, architectural thinking. By doing so, you build a codebase that is not only functional but also adaptable and scalable over time.

### 2.4 Style & Complexity Hygiene

* Automated linting, formatting, docstring enforcement.
* Complexity budgets for generated code.

LLMs are not inherently aware of a team's specific coding conventions, and a lack of oversight can quickly lead to a codebase that is a patchwork of inconsistent styles and patterns. This is why **style and complexity hygiene** is a critical cross-cutting theme for any team using LLMs. Without a consistent style, code becomes harder to read, and the cognitive load for new developers increases. If code is also unnecessarily complex, it becomes a maintenance nightmare, regardless of how it was generated.

The primary mitigation is to lean heavily on **automated linting, formatting, and docstring enforcement**. These tools should be the first line of defense in your quality gates, operating automatically in your pre-commit hooks or CI/CD pipeline. A linter can enforce a consistent style guide, catching everything from inconsistent indentation to incorrect naming conventions. A formatter, like Prettier or Black, can automatically correct stylistic issues. Requiring docstrings ensures that all functions, whether human- or LLM-generated, are well-documented, making the codebase easier to understand. This practice treats LLM-generated code as a raw input that must be immediately sanitized to meet team standards.

In addition to style, you should implement **complexity budgets for generated code**. Tools that measure metrics like cyclomatic complexity, line count, or nesting depth can be configured to fail a build if the generated code exceeds a predefined threshold. This is a powerful guardrail against the LLM's tendency to produce long, monolithic functions that are difficult to debug and refactor. When a complexity check fails, it prompts the developer to either improve the prompt or manually refactor the code into smaller, more manageable parts.

This approach not only ensures a high standard for the codebase but also helps to train developers on what "good" LLM-generated code looks like. It shifts the focus from simply accepting what the LLM outputs to critically evaluating and refining it. A developer's job in this model is not just to use the LLM, but to be the final arbiter of code quality, ensuring that the generated code aligns with the long-term health of the project.

### 2.5 Security

* Treat all LLM output as insecure by default.
* Run static security scans (injection, SSRF, crypto misuse, IAM scope).

Treating LLM-generated code as a potential security risk is a non-negotiable step. While LLMs can be helpful for finding bugs, they can also introduce subtle and dangerous vulnerabilities. The models may be trained on a vast amount of code, including insecure patterns, or they might make incorrect assumptions when generating a solution. Therefore, the most critical principle is to **treat all LLM output as insecure by default**. This means never deploying or using LLM-generated code in production without a rigorous security review.

To enforce this, you should integrate robust security checks into your development pipeline. **Run static security scans** on all generated code, looking for common vulnerabilities like SQL injection, cross-site request forgery (CSRF), server-side request forgery (SSRF), and insecure cryptographic practices. These static application security testing (SAST) tools can automatically flag suspicious patterns, such as user input being directly used in a database query or a function calling an external service without proper validation.  This automated layer acts as a crucial first line of defense.

Beyond automated scans, a **manual security review** is essential for high-risk components. For sensitive areas such as authentication, authorization, and payment processing, the LLM-generated code must be subjected to a thorough, human-led security audit. A security expert or a senior developer should review the code to ensure it adheres to best practices and does not introduce a subtle, yet critical, vulnerability that automated tools might miss. This is especially important for back-end services, where a single vulnerability can compromise an entire system.

This proactive approach to security also benefits the team's overall security posture. By treating LLM-generated code as potentially insecure, developers are forced to think critically about security at every stage. It instills a sense of shared responsibility and moves security from a final checklist item to an integral part of the development process.

### 2.6 Licensing & IP

* Run license scanners.
* Be mindful of copyright/data exposure in external APIs.

The intellectual property (IP) and licensing implications of using LLMs for code generation are complex and fraught with legal uncertainty. A core concern is that the LLM's training data may include open-source code with various licenses, including copyleft licenses. This means the generated code could potentially contain fragments of licensed code without the necessary attribution or compliance. To mitigate this, a multi-layered approach is required, starting with a fundamental awareness that the generated code is not a blank slate but a derivative work of its training data.

A crucial first step is to **run license scanners** on all generated code. Tools designed for software composition analysis (SCA) can scan your codebase for snippets that match known open-source projects. This helps to identify potential licensing violations before the code is merged. If a scanner flags a potential issue, the developer must investigate whether the generated code is a direct copy or merely coincidental. This process helps to enforce a policy of compliance and prevents the accidental introduction of code with restrictive licenses into a proprietary codebase.

You must also be mindful of **copyright and data exposure when using external APIs**. When using a public LLM API, any proprietary or sensitive code you input can potentially be used as future training data, exposing your organization's intellectual property. This is why many companies have strict policies against feeding sensitive data into public LLMs. The most effective mitigation is to use an LLM that is either self-hosted or provided by an enterprise-grade service with a contract that guarantees your data will not be used for training.

Another important aspect is the question of **authorship and ownership**. In many jurisdictions, copyright requires human authorship. This raises a complex legal question: who owns the code that is predominantly generated by an LLM? While this is still a developing area of law, a pragmatic solution is to ensure clear human involvement. By documenting the prompts used and the human-led refinements and edits to the generated code, you can establish a clear claim of human authorship, which is crucial for protecting your intellectual property.

Ultimately, the goal is to shift from a reactive stance—addressing licensing issues after they are discovered—to a proactive one. This involves establishing a clear organizational policy on LLM usage, providing training on the risks, and using the right tools to enforce compliance from the moment code is generated. This creates a safer environment where the benefits of LLM-assisted coding can be realized without exposing the organization to significant legal and IP risks.

### 2.7 Vendor Lock-in

* Treat models like compilers.
* Pin versions, stage rollouts, and monitor regressions.

Relying on a single LLM provider for your coding needs can lead to significant **vendor lock-in**, making it difficult and costly to switch models in the future. As the LLM market evolves, new, more capable, or more cost-effective models may emerge, but if your entire workflow is tightly coupled to one provider's API, you lose the flexibility to adapt. This can stunt innovation and prevent you from leveraging the best tools available.

The most effective way to mitigate this is to **treat LLMs like compilers** or any other external dependency. Your codebase should be designed with an abstraction layer that interacts with the LLM API, rather than calling the API directly throughout your code. This architectural pattern, often referred to as a "model abstraction layer" or "LLM gateway," allows you to swap out the underlying model without a major refactor. By standardizing your internal prompts and API calls, you can easily point your system to a different provider—or even a self-hosted open-source model—if needed.

To maintain stability and control, you should **pin model versions, stage rollouts, and monitor regressions**. Just as you wouldn't update your compiler or a critical library without testing, you should treat LLM version updates with the same caution. Pinning a specific model version ensures that your generated code remains consistent over time. When a new version is released, you should roll it out in a staged manner, starting with non-critical tasks and monitoring its output closely for any regressions or unexpected behavior.

This approach transforms the LLM from a monolithic service into a modular component of your tech stack. It provides the strategic flexibility to choose providers based on performance, cost, security, or feature set, rather than being beholden to the whims of a single vendor. It empowers your organization to make data-driven decisions about which LLMs to use, ensuring your tech stack remains resilient and adaptable in a rapidly changing landscape.

### 2.8 Observability

* Generated code must include logging, metrics, and tracing hooks.

Code generated by an LLM is often a black box; it might be syntactically correct and pass basic tests, but without a deep understanding of its inner workings, it can be impossible to debug in a production environment. This is a critical issue for **observability**, as it can lead to silent failures, performance bottlenecks, and a general lack of insight into system behavior. A developer cannot fix what they cannot see, and relying on opaque code introduces significant operational risk.

The key mitigation is to enforce that all **generated code must include logging, metrics, and tracing hooks**. The prompt or the tooling layer should explicitly instruct the LLM to instrument the code. For logging, this means including clear, informative log messages at key points, such as function entry/exit, error conditions, and important state changes. For metrics, it involves exposing counters for events and gauges for resource usage. For tracing, it means wrapping key operations in spans that can be used by a distributed tracing system like OpenTelemetry. This ensures that the generated code is not just functional but also transparent and debuggable from the moment it is deployed.

This practice also fosters a culture of observability within the team. By requiring LLM-generated code to be instrumented, you set a standard that all code should be observable. It makes it easier to monitor the performance of your systems, diagnose issues quickly, and understand the real-world impact of a change. This transforms the LLM from a simple code generator into a partner in building robust, production-ready systems.

### 2.9 Cost & Sustainability

* Cache outputs; use smaller models for routine tasks.
* Monitor API spend and environmental impact.

While the benefits of using LLMs for coding can be significant, the financial and environmental costs can be substantial. API calls to large models are not free, and the computational resources required for both training and inference consume vast amounts of energy. Ignoring these factors can lead to an unexpected ballooning of a project's budget and a larger environmental footprint. Therefore, a strategic approach to managing **cost and sustainability** is essential for any organization adopting LLM-assisted workflows.

One of the most effective cost-management strategies is to **cache outputs and use smaller, more efficient models for routine tasks**. For common prompts, such as generating a test skeleton or a simple boilerplate function, the output is often the same. Caching the generated code after the first request can eliminate redundant API calls and save money. Furthermore, not every task requires a massive, state-of-the-art model. For simpler tasks like documentation generation or syntax correction, a smaller, more specialized model can be used. This tiered approach ensures you are using the right tool for the job, balancing performance with cost.

To ensure long-term sustainability, you should **monitor API spend and environmental impact**. Set up dashboards and alerts to track your organization's LLM API usage in real-time. This provides visibility into where costs are being incurred and helps identify opportunities for optimization. While environmental impact is harder to measure, being mindful of the energy consumption of LLM providers can inform your strategic decisions, encouraging the use of providers who prioritize renewable energy and efficient data centers.

This proactive management of costs and sustainability not only benefits the company's bottom line but also aligns with corporate responsibility goals. It shifts the conversation from a blind adoption of new technology to a thoughtful and measured integration into the existing workflow.

### 2.10 People & Skills

* Developers need prompt fluency and "LLM code review" skills.
* Prevent skill atrophy: pair juniors with mentors.
* Form an LLM working group to codify practices.

The introduction of LLMs into the software development workflow is not just a technological shift; it's a profound change in the roles and skills of the people on a team. Successfully integrating this technology requires a deliberate focus on **people and skills** to ensure the team can harness its power while managing the inherent risks. Without this focus, you risk creating a team that is reliant on tools without the critical thinking to use them effectively.

Developers need to evolve from simply writing code to becoming skilled "prompt engineers" and "LLM code reviewers." **Prompt fluency** is the ability to craft precise, detailed prompts that guide the LLM to generate high-quality, relevant code. This requires an understanding of the model's capabilities and limitations, as well as a new kind of creative and analytical thinking. Equally important are **"LLM code review" skills**, which involve the ability to critically evaluate and validate the generated output. This is not a simple syntax check; it's a deep dive into the code's logic, security implications, and adherence to architectural principles, ensuring that the code is not just plausible but correct and maintainable.

There is a real risk of **skill atrophy**, particularly for junior developers who might rely on LLMs to generate boilerplate code without fully understanding the underlying concepts. To prevent this, a structured approach is needed. One effective mitigation is to **pair junior developers with mentors**. The mentor can guide the junior developer on when to use an LLM and when to write the code themselves, helping them to build a strong foundation of fundamental skills. This mentorship ensures that the LLM is a learning tool, not a crutch.

Finally, a dedicated effort is needed to formalize best practices across the organization. You should **form an LLM working group to codify practices**. This cross-functional group, including senior engineers, security professionals, and engineering managers, can create and maintain a living document of guidelines for LLM usage. This includes approved models, prompting best practices, and the required quality gates for LLM-generated code. This creates a shared knowledge base and ensures that the entire organization is aligned on how to use LLMs safely and effectively.

## 3 Architectural Suitability & Strategic Fit

LLM suitability depends not just on architecture, but on **contextual factors**:

* **Contract maturity** — are APIs/interfaces formally specified and testable?
* **Performance sensitivity** — does the system have strict latency/throughput requirements?
* **Security sensitivity** — would a subtle bug introduce major risk?

The effectiveness of LLM-assisted coding is not uniform across all software projects; its suitability depends on a combination of the application's architecture and several key **contextual factors**. A one-size-fits-all approach to integrating LLMs is likely to fail. Instead, engineering leaders must evaluate their specific systems and workflows to determine where LLM assistance will provide the most value while minimizing risk. The primary contextual factors to consider are the **contract maturity**, **performance sensitivity**, and **security sensitivity** of a given application or component.

An application with high **contract maturity**, where APIs and interfaces are formally specified and testable (e.g., via an OpenAPI spec), is an ideal candidate for LLM assistance, particularly in the "spec-to-scaffold" mode. The explicit, machine-readable contracts act as a guardrail against hallucinations and ensure the generated code is both correct and reproducible. Conversely, a system with a low degree of contract maturity, perhaps relying on implicit or undocumented interfaces, is more susceptible to the risks of LLM-generated code.

For systems with high **performance sensitivity** and strict latency or throughput requirements, the use of LLMs requires a high degree of caution. While an LLM can generate functional code, it may not produce the most performant or idiomatic solution. A generated algorithm might be a simple, O(n²) implementation where a human-written O(log n) solution is required. Similarly, in high-stakes environments where **security sensitivity** is paramount—such as in finance or healthcare—a subtle bug introduced by an LLM could have catastrophic consequences. In these cases, even with robust security scans, the risk may outweigh the speed benefits.

Ultimately, the decision to use LLMs should be a strategic one, tailored to the unique characteristics of each project. To provide a clear and concise guide for this decision-making process, the following table compares different application architectures and their suitability for LLM-assisted coding across these key contextual factors.

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

This section highlights the most common and dangerous failure modes that arise from LLM-assisted coding and provides quick, actionable counters. These failure modes are universal, transcending individual coding modes to affect the entire development process. A proactive approach to these issues is essential for any team looking to integrate LLMs safely and effectively.

The first and most pervasive failure mode is **hallucinated APIs/flags**. LLMs can confidently invent functions, flags, or even entire library features that do not exist. This leads to code that looks correct but fails to compile or run. The quick counter is to require the model to cite specific, versioned documentation for any API it uses and to automatically verify this with a compile-and-typecheck step in your continuous integration (CI) pipeline. This process turns a plausible-looking snippet into a validated piece of code.

Another common and dangerous pitfall is **shallow testing**. LLMs often produce "happy path" tests that only validate the most straightforward use cases, giving a false sense of security. To counter this, a developer should always ask the LLM for adversarial and boundary cases. Where applicable, you should also add fuzz or property testing. This forces the generated code to be robust against unexpected inputs, ensuring it holds up in a real-world environment.

**Concurrency bugs** are another subtle failure mode that LLMs are particularly bad at. The complexity of race conditions and shared state is beyond the scope of most LLM-generated code. The best defense is to prefer pure functions, isolate all side effects, and add stress tests to your CI pipeline. Furthermore, require the LLM to include explicit timeouts and cancellation semantics in its code, providing a safety net for long-running or stalled operations.

**Error handling holes** are a frequent issue. An LLM might generate code for a function's successful path but completely omit the error-handling logic. This can lead to silent failures or unhandled exceptions in production. The countermeasure is to standardize error envelopes across your codebase and explicitly request the LLM to generate failure-path tests by default. This ensures that a function’s error handling is as robust as its core logic.

Beyond the code itself, **cost and resource management** is a major concern. The financial and environmental costs of using large LLMs can be significant, especially with high-volume usage. This should be managed by using smaller, more efficient models for simpler tasks, caching generations to avoid redundant API calls, and meticulously monitoring your API spend and compute usage.

A more human-centric failure mode relates to the **"LLM as a Partner" skill set**. Developers who simply copy and paste LLM output without critical thought can introduce significant technical debt. To prevent this, developers need to become proficient at prompt engineering, managing conversational context, and understanding model limitations. This new skill set is a major consideration for hiring, training, and team dynamics, shifting the focus from rote coding to strategic problem-solving.

**Prompt injection and data exfiltration** are serious security threats. A malicious actor could embed a hidden command in a bug report, causing an autonomous agent to perform an unauthorized action. To counter this, you must sanitize all inputs, strip generated prompts from untrusted code, and never run a suggested shell command without a thorough human review. This is especially important for agentic workflows.

**Dependency bloat/version drift** is a common consequence of relying on LLMs. The model might suggest using a new or outdated library, leading to an ever-expanding node_modules folder or a collection of inconsistent library versions. To mitigate this, enforce allow-lists for dependencies, run regular dependency pruning, and pin and audit all versions to ensure your project's dependencies are secure and manageable.

Finally, a related failure mode is **performance regressions**. An LLM might generate an inefficient algorithm that works for small inputs but grinds to a halt in production. To combat this, you must capture baseline performance tests and add performance budgets to your CI. Requesting complexity or performance justifications in your pull request templates can also prompt the developer to think critically about the code's efficiency.

## 5 Process Patterns that Make LLM Code Durable

* **Contract-first:** OpenAPI/GraphQL/Protobuf/JSON Schema as the "north star." Generate servers/clients/tests from it.
* **Example-driven:** Turn user stories into Given/When/Then examples; have the model derive tests first, then code to fit.
* **Generation boundaries:** `/generated/**` is machine-owned; hand code uses interfaces only. Regeneration is safe and reviewable.
* **Repo maps:** Feed the model a curated repo map (key files, contracts, constraints) instead of the whole tree; reduces wrong context.
* **Model as reviewer, not decider:** Bots can block on objective checks; humans approve architecture.
* **Metadata everywhere:** PRs must include the exact prompts + model/version used; CI saves them as artifacts.
* **Org-approved templates:** Maintain your own golden templates/cookbooks; don’t let the model invent structure each time.

The long-term durability and maintainability of LLM-generated code depend less on the specific tools and more on the processes and patterns a team adopts. Without these, a codebase can quickly become a brittle, unmanageable mess. This section outlines the key process patterns that transform LLM-assisted coding from a tactical shortcut into a strategic, repeatable, and scalable practice. These patterns act as guardrails, ensuring that LLM-generated code becomes a well-integrated, lasting part of a project.

The **contract-first** pattern is the most important of these. The principle is simple: define the contract—the API specification, data schemas, or component interfaces—before writing any code. Tools like OpenAPI, GraphQL, Protobuf, or JSON Schema become the "north star" for development. You can then use the LLM to generate the servers, clients, and tests directly from this contract. This ensures that the generated code is always in lockstep with the agreed-upon behavior, providing a stable, verifiable foundation that protects against the LLM's tendency to hallucinate.

An **example-driven** approach is another powerful pattern. Instead of using a vague prompt, you should formalize user stories into clear, testable examples using a format like Given/When/Then. The first step is to have the LLM derive tests from these examples. Only after the tests are written and accepted do you use the LLM to generate the code that makes those tests pass. This flips the traditional workflow, ensuring that the code's functionality is validated by a human-written specification before a single line of implementation is committed.

To prevent the "regeneration debt" discussed earlier, you must establish clear **generation boundaries**. A simple and effective pattern is to designate a specific directory, such as `/generated/`, as machine-owned. All LLM-generated code resides here and is never manually edited. Your hand-written code should only interact with this generated code through well-defined interfaces. This makes regeneration a safe and reviewable process, as any changes can be easily seen in a PR without fear of overwriting human-authored logic.

For complex projects, a **repo map** pattern can be highly effective. Instead of feeding the entire codebase to the LLM, which can lead to context overload and incorrect assumptions, you feed it a curated map of the repository. This map includes links to key files, contracts, and architectural constraints. This provides the LLM with a focused, relevant context, significantly reducing the likelihood of generating code that violates project-specific patterns or references outdated information.

Another crucial pattern is to use the **model as a reviewer, not a decider**. While autonomous agents promise to automate the entire development cycle, a safer and more pragmatic approach is to use the LLM for objective, mechanical checks. A bot can be configured to block a PR if it fails a linter or a test, but a human must always be the final arbiter of architectural decisions and strategic fit. This human-in-the-loop pattern ensures that the final code reflects a shared vision and not just a machine's best guess.

To create a clear audit trail, a **metadata everywhere** pattern is essential. Every pull request that contains LLM-generated code must include a block of metadata detailing the exact prompts used, the model and version, and any other relevant parameters. This metadata should also be saved as an artifact in your CI/CD pipeline. This provides a clear provenance for every line of generated code, making it auditable and reproducible.

Maintaining a library of **org-approved templates** or "golden patterns" is another powerful strategy. Instead of letting the LLM invent a new structure for a service or a component each time, you provide it with a pre-approved template that codifies your organization's best practices. This ensures consistency across the codebase, simplifies the prompt, and significantly reduces the risk of style or architectural drift.

## Conclusion: From Tool to Partner

The adoption of LLMs in software development is not a passing trend but a fundamental shift in how we build and maintain software. The journey begins with understanding the various "coding modes," from simple autocomplete to fully autonomous agents, each with its own set of benefits and risks. As we've seen, a careless approach can introduce significant technical debt, security vulnerabilities, and maintainability headaches. However, by embracing a strategic and disciplined framework, these powerful tools can be integrated safely and effectively.

The key to success lies in moving beyond treating the LLM as a mere novelty and instead, viewing it as a new kind of coding partner. This partnership is built on a foundation of robust processes and clear principles: a contract-first mindset, a commitment to observability and security, and a continuous focus on the skills and practices of your people. The goal is to elevate the developer's role from a code writer to an architect and a validator, where human judgment, critical thinking, and creativity remain paramount.

By applying the cross-cutting themes, understanding architectural suitability, and implementing the durable process patterns outlined in this document, organizations can harness the speed and scale of LLMs without compromising the long-term health of their codebase. This proactive approach ensures that LLMs become a force multiplier for the engineering team, enabling them to build more reliable, scalable, and innovative software than ever before. The future of software development is not a world without human developers, but one in which they are empowered to focus on the problems that truly matter.

## Prompts

The following are the most significant prompts that were used to generate the content on this page. Note that the content for [`using_llms_in_software_coding_points.md`](using_llms_in_software_coding_points.md) was generated first, after which the generated content is used for this document.

**Gemini 2.5 Flash:**

The following are the content headings for a document titled, "Using LLMs in Software Coding: Modes, Risks, and Mitigations." It provides a mode-by-mode overview of how developers use LLMs in software engineering, what tends to go wrong, and how to mitigate long-term maintainability risks. It is intended for CTOs, engineering managers, and developers evaluating the role of LLMs in their workflow.

I have included a brief description of the intended content for some of the sections and their subsections.

1 Coding Modes

This section contains subsections that discuss each of the LLM-assisted coding modes. Each subsection discusses what the coding mode is good for, common issues and maintainability risks, and mitigations for them.

1.1 “Vibe coding” (exploratory, natural-language prompts; low up-front spec)

1.2 Assistant-as-autocomplete (inline suggestions while you type)

1.3 Spec-to-scaffold (generate a service/module from a structured brief)

1.4 Code review / PR assistant

1.5 Bug-hunting / “why is this failing?”

1.6 Translation & refactoring (e.g., Python→Go, class→hooks, v2→v3 API)

1.7 Test generation (unit, property-based, fuzz inputs)

1.8 Data/SQL & analytics coding

1.9 Front-end UI (components, forms, state wiring)

1.10 Back-end endpoints & services

1.11 Infra as Code / pipelines (Terraform, CI, Dockerfiles)

1.12 Autonomous/agentic coding (tickets→PRs end-to-end)

1.13 Documentation & examples

2 Cross-Cutting Themes

This section lists, in subsections, mitigations that are common across the earlier coding modes.

2.1 Contract-First Development (Primary Safeguard)

2.2 Determinism & Provenance

2.3 Boundaries & Regeneration

2.4 Style & Complexity Hygiene

2.5 Security

2.6 Licensing & IP

2.7 Vendor Lock-in

2.8 Observability

2.9 Cost & Sustainability

2.10 People & Skills

3 Architectural Suitability & Strategic Fit

This section examines application types and architectures, and their suitability for LLM-assisted coding. The examination includes contextual factors such as application contract maturity, performance security, and security sensitivity.

4 Process & People: Cross-Cutting Failure Modes

This section highlights the most common and dangerous failure modes and provides quick counters. These are universal to all modes of LLM-assisted coding.

5 Process Patterns that Make LLM Code Durable

This section lists the process patterns that help make LLM code durable and maintainable over time.

6 One-Page Checklists

This section provides brief checklists of what to do before code generation by LLMs, after code generation, and the ongoing maintenance of the generated code.

6.1 Before generating

6.2 After generating

6.3 Ongoing maintenance

This concludes the document.

Remember the above and ask me what I want to do next.

**Gemini 2.5 Flash:**

I will be asking you to write a number of paragraphs of content for a specific section, in which you write for that section and not for any other sections. The content will be within the context of the content headings I asked you to remember earlier.

I will also include some initial content, which you will use as your seed content for the section. You should add additional relevant content for that section beyond the initial content.

Ask me for the section to write.

**Gemini 2.5 Flash:**

Write up to 7 paragraphs for section 1.1.

Seed content:

* **Good for:** quick spikes, toy apps, UI mockups, simple CRUD, tiny CLIs.
* **Common issues:** hidden assumptions; shaky edge-case handling; silent API mismatches; code that “looks right” but fails on integration.
* **Maintainability risks:** no source of truth beyond the chat; prompt-drift (can’t reliably regenerate); inconsistent patterns/styles.
* **Mitigations:**
  * Freeze a lightweight spec (user stories + examples).
  * Convert to tests before expanding.
  * Check in the exact prompt + model/version.
  * Require a **formalized human refactor pass** with documented checklist and time budget.

**Gemini 2.5 Flash:**

Write 2 paragraphs for section 1 Coding Modes. This should be an introduction for all the subsections within this section. As a reminder, the subsections are:

1.1 "Vibe coding" (exploratory, natural-language prompts; low up-front spec)

1.2 Assistant-as-autocomplete (inline suggestions while you type)

1.3 Spec-to-scaffold (generate a service/module from a structured brief)

1.4 Code review / PR assistant

1.5 Bug-hunting / “why is this failing?”

1.6 Translation & refactoring (e.g., Python→Go, class→hooks, v2→v3 API)

1.7 Test generation (unit, property-based, fuzz inputs)

1.8 Data/SQL & analytics coding

1.9 Front-end UI (components, forms, state wiring)

1.10 Back-end endpoints & services

1.11 Infra as Code / pipelines (Terraform, CI, Dockerfiles)

1.12 Autonomous/agentic coding (tickets→PRs end-to-end)

1.13 Documentation & examples

**Gemini 2.5 Flash:**

Come up with a section heading and write a few paragraphs to wrap-up and conclude this document. As a reminder, here are the main sections:

1 Coding Modes

2 Cross-Cutting Themes

3 Architectural Suitability & Strategic Fit

4 Process & People: Cross-Cutting Failure Modes

5 Process Patterns that Make LLM Code Durable
