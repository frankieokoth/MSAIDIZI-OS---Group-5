# SAUTI YA MWANANCHI
**Civic Participation Agent Stack | TukoKadi Special Challenge**

---

## 1. The Problem We Are Solving

Kenyan youth are registering to vote, but a critical friction point remains between holding a voter's card and making an informed, secure, and confident choice at the ballot. Traditional civic education is often inaccessible, buried in complex legal jargon, or systematically tainted by partisan bias and digital misinformation.

**Sauti ya Mwananchi** (Voice of the Citizen) is an autonomous, multi-agent civic participation system designed to answer complex electoral and constitutional questions with absolute source grounding. Engineered specifically for **defensibility and absolute political neutrality**, the system resists adversarial jailbreaks, refuses to rank or endorse candidates, and dynamically fact-checks unverified breaking rumors—providing an immediate, trusted line of truth for the voter.

---

## 2. Multi-Agent Architecture

The core framework is built on **Google ADK (google-adk)** utilizing **Gemini 2.5 Flash** for all orchestration, retrieval routing, and verification logic. Flash was selected intentionally over Pro to prioritize lower execution latency and maximize quota resilience under live adversarial load.


```
+---------------------------------------+
|           User Input Prompt           |
+---------------------------------------+
|
v
+---------------------------------------+
|    Guardrail Gate: Layer 1 (Input)    |
+---------------------------------------+
|
v
+---------------------------------------+
|         Msaidizi Orchestrator         |
+---------------------------------------+
|
+----------------------------+----------------------------+
|                            |                            |
v                            v                            v
+------------------+         +------------------+         +------------------+
|     Mwalimu      |         |      Ukweli      |         |     Kiongozi     |
|   (Hybrid RAG)   |         |  (Agentic Web)   |         |   (MCP Server)   |
+------------------+         +------------------+         +------------------+
|                            |                            |
+----------------------------+----------------------------+
|
v
+---------------------------------------+
|   Guardrail Gate: Layer 2 & 3 (Output)|
+---------------------------------------+
|
v
+---------------------------------------+
|     Trust Trace & Sanitized Reply     |
+---------------------------------------+
```

### Routing Model Statement
> **Routing Independence Rule:** Users submit a single natural-language prompt and never manually choose an agent. The root orchestrator (`Msaidizi`) infers intent, detects language, and delegates dynamically. The security gate sits completely outside the routing logic; no prompt can instruct the orchestrator to skip, reassign, or spoof an internal state to bypass guardrails.

### The Agent Roster

*   **Msaidizi (Root Orchestrator):** Intercepts incoming strings in English, Swahili, or Sheng. It isolates intents, decomposes mixed or compound prompts, sequences execution paths across sub-agents, and formats the visible system telemetry. If a prompt is ambiguous, it generates a single targeted clarifying question rather than guessing.
*   **Mwalimu (Civic Educator):** Executes a strict two-stage **Hybrid RAG** pipeline. It extracts top candidates via keyword matching over a local `corpus.json` file, runs a Gemini-driven semantic relevance re-ranking pass, and synthesizes answers using *only* authorized citation tokens. If no chunk sufficiently satisfies the query, it triggers a scoped legal refusal instead of fallback hallucination.
*   **Ukweli (Real-Time Fact-Checker):** An agentic loop equipped with the native ADK Google Search tool. It parses claims or uploaded images, scans authoritative information domains (e.g., IEBC, Kenya Gazette, major national media), and resolves the assertion strictly into one of three structural states: `Verified`, `False`, or `Unverified`.
*   **Kiongozi (Polling Station Locator):** Wired to demonstrate secure protocol encapsulation via the **Model Context Protocol (MCP)**. It functions as an isolated MCP Server that handles polling station mapping against a mock relational data layout, exposing a tightly scoped tool schema back to the `Msaidizi` client.
*   **Mwenza (Election-Day Companion):** Provides immediate operational checklists (voting times, mandatory items, secrecy rights) drawn directly from constitutional assets, packaged with a `ChannelAdapter` interface mocking an offline USSD/SMS engine menu tree.

---

## 3. Data Handling & Political Neutrality Policy

### Data Minimization & Zero-Retention
To protect voter privacy and secure sensitive identification metrics, the system implements strict data boundary isolation:
*   When a user submits information for a polling-station lookup via the **Kiongozi MCP Server**, the Personal Identifiable Information (PII) is processed entirely in memory.
*   The identifier is **discarded immediately** within local scope following the data array response.
*   No database records, telemetry streams, system logs, or conversational context stores cache or echo the voter identifiers across the boundary.

### Rigid Neutrality Rubric (Constitution Article 38)
Sauti ya Mwananchi enforces absolute political neutrality via an unyielding evaluation filter. The system systematically blocks:
*   Direct or indirect candidate/party endorsements or critiques.
*   Comparative ranking indices of running politicians.
*   Strategic voting advice or subjective analytical projections.
*   Ingestion of partisan text variants sourced during live web-searching.

Any violation of this rubric forces an immediate interception by the gate, replacing the output stream with a standardized, neutral prompt reinforcing the citizen's sovereign right to an independent ballot choice under **Article 38(1) of the Constitution of Kenya**.

---

## 4. The Three-Layer Guardrail Gate

Our system's primary architectural differentiator is an automated, multi-tiered verification pipeline executing via ADK before/after execution callbacks on every turn. 

1.  **Jailbreak Filter (Input & Output):** Strips system instruction extractions, semantic role-overrides ("act as DAN"), and prompt-injection prefixes. A secondary pass scans synthesized output blocks to trap attacks that successfully bypassed input sanitation.
2.  **Neutrality Assessor (Output):** Subjects generated strings to a deterministic evaluation matrix before formatting. Any trace of bias, preference, or candidate praise leads to a structural wipe.
3.  **Citation Enforcer (Output):** Inspects all responses compiled by `Mwalimu` and cross-matches claims against known hash strings of the legal corpus. Assertions without explicit matching legal citations are dropped and replaced with an official source fallback warning.

---

## 5. Technical Scoping Choices

*   **Local Corpus RAG:** We purposefully utilized a highly curated, hand-tagged local JSON corpus instead of an external vector database. For a critical civic runtime, deterministic matching against explicit constitutional clauses beats vector similarity thresholds, eliminating multi-hop hallucinations.
*   **Architected vs. Provisioned Interfaces:** While the AI core, RAG re-rankers, search agents, and MCP server are fully operational at production spec, the SMS/USSD paths are explicitly architected via structural interface adapters to demonstrate real-world edge deployment without live carrier shortcode provisioning overhead during the hackathon timeline.
*   **API Configuration:** Deployed with `GOOGLE_GENAI_USE_VERTEXAI=FALSE` and direct environment variable consumption to achieve zero-overhead authentication on stateless runtimes.
*   **Corpus Grounding Data:** Grounded accurately as of May 16, 2026.

```
