# AI PM Agents — LangChain / LangSmith

Production-quality AI agents built as a portfolio for the **Product Manager, LangSmith** role at LangChain. Each notebook is a working prototype, not a tutorial — built to demonstrate how I think about agent observability, evaluation, and reliability.

---

## What's in here

### 1. `research-agent.ipynb` — Foundations & Career Co-Pilot
Six agents covering the full agent engineering surface:

- **Weather agent** — basic ReAct loop, tool calling, traced end-to-end
- **Multi-tool router** — search + calculator + utility tools, demonstrates tool selection
- **Failing-on-purpose agent** — intentional failure modes (timeouts, rate limits, garbage outputs) to show what production debugging actually looks like in LangSmith
- **Evaluation harness** — 5-example dataset, custom `city_mentioned` evaluator, scored runs
- **Prompt A/B test** — terse vs. friendly prompt variants compared side-by-side (counterintuitive finding: verbose prompt was 3x more expensive with zero quality lift)
- **PM Career Co-Pilot** — reads my actual Gmail, Notion, and Google Drive to synthesize where I am in my AI PM job search, what's strong, what's weak, what to do this week

### 2. `mood_cartographer.ipynb` — Contextual Music Intelligence
Prototype of a SiriusXM concept I pitched to Sierra AI. The agent reads weather, calendar, and temporal signals, maps them to one of 6 mood states (Focus/Drive/Workout/Wind-down/Melancholic/Social), and returns a structured playlist concept grounded in a personal taste profile.

**Why it matters:** demonstrates the cold-start solution for personalization agents — the system works on day 1 with a stated profile, and improves once real listening data plugs in.

---

## Architecture decisions worth flagging

1. **Honest uncertainty.** Every Mood Cartographer output includes a `confidence` score. When a tool failed (calendar timed out), the agent flagged it and reduced confidence rather than fabricating.
2. **Structured outputs everywhere.** No vibes summaries. Every agent returns JSON-shaped data so downstream consumers can act on it.
3. **Real bugs surfaced.** The Career Co-Pilot trace exposed a classic newline-in-tool-input bug — diagnosable in seconds because of LangSmith's per-tool-call view. Fixed at the tool boundary, not per-call.
4. **Traces as the deliverable.** Every run lands in LangSmith. The traces *are* the portfolio artifact, not the code.

---

## Stack
- **LangChain** (ReAct agents, AgentExecutor, Hub prompts)
- **LangSmith** (tracing, datasets, experiments, evaluators)
- **Claude Sonnet 4.6** (Anthropic API)
- **Open-Meteo** (weather), **Google APIs** (Gmail/Drive/Calendar), **Notion API**, **DuckDuckGo Search**

---

## Linked artifacts
- [Sierra AI pitch deck](./docs/sierra-pitch.pdf) — the original SiriusXM concept the Mood Cartographer prototypes
- LangSmith project: `mood-cartographer`
- LangSmith project: `pr-formal-oleo-17`

---

Built by Siddharth Rallabandi — currently AI Platform Engineering @ US Bank, transitioning to AI Product Management.
[LinkedIn](https://linkedin.com/in/siddharthrallabandi) · rallabandisiddharth@gmail.com
