FactCheck: an AI agent that verifies news claims with sources
Team: Karim Aidos, Karabai Eldos  
1. Problem and user
False claims spread on social media and messengers faster than readers can check them, and finding reliable sources takes time and skill. Our user is an ordinary reader or student who sees a factual claim (a number, a date, an event) and wants a quick answer backed by evidence they can open and verify. The scope is factual, checkable claims in English only. Opinions, predictions and value judgments are out of scope.
2. AI feature and evaluation
The user enters a claim. A single LLM agent runs a tool-use loop: it plans search queries, retrieves pages, extracts supporting or contradicting quotes, and decides when it has enough evidence. It returns a verdict (true / false / not enough data) with links and short quotes.
Tools: (1) web search, (2) page reader for open pages, (3) quote extraction.
Evaluation:
Test set: 50+ claims, about 17–20 per verdict, a mix of published fact-checks and claims we write ourselves from unambiguous dated facts. At least one third concern recent events, so a model without search cannot answer from memory. The “not enough data” class contains claims that cannot be verified from open sources.
Labeling: both members label each claim independently. Disagreements are removed from the set.
Metrics: verdict accuracy (overall and per class, especially “not enough data”); link quality (share of cited links that actually support the verdict, checked by hand on a sample); cost and time per claim.
Baseline: the same LLM, same prompt style, one pass with no search. Results are reported side by side.
3. Agentic tools
Development: [OpenCode with a free model to start; Claude Code Pro for one month if free daily limits block us]. Any switch is recorded in DECISIONS.md. We write PLAN.md before code, use a stronger model for planning and a cheaper one for routine edits.
In the product: LLM API [Claude API] with a custom agent loop (no heavy framework), so each of us can explain every line.
Cost: paid usage is limited to the API for evaluation runs (our estimate: $20–50 in total) and optionally one month of a paid coding agent. Search and everything else is free.
4. MVP and stretch goals
MVP: one claim goes end to end through a CLI: search, read, extract quotes, verdict, links. PLAN.md, AGENTS.md and the first agent logs exist.
Stretch goals:
Full evaluation on the 50+ claim set with the no-search baseline and per-class results.
A simple web page for the demo (Streamlit or similar), or source-reliability weighting if time allows.
5. Data and external APIs
Search API: [Tavily], free tier (1,000 credits per month, no card required). We will create a key. Responses are cached on disk to save credits and make runs reproducible.
Page reading: open web pages only, via standard HTTP requests. No bypassing of paywalls or site protection.
LLM API: [provider], key stored in an environment variable, never in the repository.
Test claims: public fact-checking sites and open sources, labeled by us. No special access needed, no personal data collected.
6. Who does what
Karim Aidos: agent loop, tool integration (search, page reader, quote extraction), verdict logic, CLI.
Karabai Eldos: test set and labeling protocol, evaluation script and metrics, no-search baseline, README, tests.
Both: label claims independently, review each other’s code, keep their own agent logs and commits. We swap tasks midway (for example, Eldos extends the agent, Aidos runs the final evaluation) so each of us can explain the whole system at the defense.


