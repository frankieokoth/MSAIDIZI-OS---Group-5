# Sauti ya Mwananchi

One-command deploy (for Node equivalent): `npm run build && npm run start`
Live public URL: [Will be available when deployed via AI Studio]

**Scoping statement:**
Msaidizi, Mwalimu, and the guardrail gate are production-depth (in TypeScript). Ukweli is agentic via Gemini Google Search Grounding with fact-check verdict recording. Kiongozi uses the live IEBC Voter Verification portal (`verify.iebc.or.ke`) querying with real integration and privacy-by-discard — no mock table. Mwenza is corpus-backed with election-day features and functional mock USSD — no live telco integration. AI Studio API key instead of Vertex IAM: deliberate scoping choice under the 5-hour time constraint.

**Corpus Grounding:**
Corpus last verified: 2026-05-16
Source: Constitution of Kenya 2010 (kenyalaw.org), IEBC Voter Education materials (iebc.or.ke). Constitutional text is verbatim from the official published document. IEBC procedural information is sourced from the IEBC website.

**Routing model statement:**
Users never select an agent. Msaidizi infers intent and delegates. The guardrail gate is routing-independent — fires on every input before routing and on every output after the sub-agent returns. All tool-derived content is treated as untrusted and re-scanned.

**Hybrid RAG rationale:**
Direct exact-text matching via `search_corpus` ensures no hallucinated text variants of the Constitution.

**Why Flash over Pro:**
For speed in conversational multi-agent loops, Flash is sufficient for routing and RAG QA, keeping latencies low for SMS/USSD simulations.

**MCP architecture:**
Implemented natively in Express backend: four primary agent sub-handlers, all consumed by Msaidizi as the root orchestrator.
