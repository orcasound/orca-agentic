# Agentic AI for Marine Acoustic Monitoring

README draft 10/8/2025 - generated interactively by @adrmac using ChatGPT
Planning for a [5-day AI Agents Intensive Course with Google]([url](https://rsvp.withgoogle.com/events/google-ai-agents-intensive_2025)), Nov 10 - 14, 2025

**Mission:** Move Orcahello beyond a single CNN classifier into an **agentic system** that fuses multiple evidence sources (acoustics, vessel context, human reports, optional CLAP) to **reason, learn, and communicate**—resulting in higher confidence alerts, scalable funding pathways, and better contributions to science.

---

## Why this matters (now)
- **Today:** Orcahello = automated classifier pipeline (CNN → threshold → moderator check → alert). Effective, but **not adaptive** and **not context-aware**.  
- **Next:** An **agent** that cross-checks detections with vessel data, human reports, and (optionally) CLAP semantic similarity. The agent will remember recent context; explain decisions; and improve over time.

**Outcomes:** Higher confidence real-time alerts, making Orcasound more competitive for grants/sponsorship as an environmental monitoring platform, capturing potential opportunities with ports, regional and national government, shipping, and offshore energy.

---

## What this enables for Orcasound
- **Contextual confidence** (not just a CNN score): combine acoustic metrics, vessel proximity/speed, human reports, optional CLAP.  
- **Explainable alerts:** structured rationale and nearest similar events.  
- **Memory & adaptation:** short-term dedupe + long-term evidence logging; pathway to retraining.  
- **Marketability:** a concrete path to funded pilots, compliance reporting, and “insights” products (monthly acoustic health, trend reports).

---

## Related repos (how this fits)
- **`orcasound-next`** — experimental Next.js UI showing agent decisions, traces, and insights (read-only client).  
- **`ambient-sound-analysis-api`** — Python/FastAPI service for **HLS→WAV**, **PSD (banded)**, and acoustic metrics; also used for backfills/archival analysis.
- **`aifororcas-livesystem`** - production code for Orcahello CNN, sending data to the **`orcasite`** live listening app and moderator interface
- **`orca-clap`** — optional CLAP plugin that provides **semantic similarity** and **nearest-examples retrieval** (plus caption proposals) for multi-species and annotation assist.  
- **(This repo) `orca-agentic`** — the **orchestration brain** that calls those tools, fuses evidence, manages memory, and emits explainable decisions.

---

## Levels of autonomy (the arc)
- **L1 Classifier:** CNN labels chunks (where we began).  
- **L2 Pipeline:** Continuous monitoring + auto logging (where we are).  
- **L3 Multi-modal Analyzer (capstone target):** Fuse CNN + PSD/AIS + human reports (+ optional CLAP).  
- **L4 Adaptive Agent (near-term):** Learn from moderator feedback; generate stakeholder summaries.  
- **L5 Ecosystem:** Specialized agents (acoustics, vessels, movement, comms) coordinating via shared contracts.

---

## Capstone scope (5-day intensive)
- **Tools:** integrate PSD/AIS + human reports; optional CLAP as a “second opinion.”  
- **Fusion:** produce a **ConfidenceBreakdown** with weights and rationale.  
- **Memory:** short-term windowing + Postgres long-term trace store.  
- **Observability:** structured logs, trace viewer, basic metrics.  
- **Handoff:** emit a clean JSON payload other services (or a “Communicator agent”) can turn into human-readable alerts.

---

## API (contract-first, lightweight)
This repo returns a **Detection** with an explainable breakdown. Example:

```json
{
  "id": "det_12345",
  "stream": "rpi_sunset_bay",
  "start": "2025-09-25T13:00:00Z",
  "end": "2025-09-25T13:00:30Z",
  "scores": {
    "cnn": 0.82,
    "psd_snr": -12.3,
    "ais_context": 0.41,
    "human_reports": 0.66,
    "clap_sim": 0.75
  },
  "confidence_final": 0.81,
  "rationale": [
    "High CNN score",
    "Moderate human corroboration nearby",
    "Within orca band but elevated vessel noise",
    "Similar to 3 prior confirmed events"
  ],
  "trace_id": "tr_7890"
}
```

Minimal endpoints (FastAPI):
- `POST /score` → Detection + ConfidenceBreakdown + `trace_id`  
- `GET /trace/{trace_id}` → full decision path (tool calls, features, weights)  
- `GET /metrics` → basic counters/histograms (Prometheus-compatible)

> **Contracts:** JSON examples live in `contracts/Detection.json` and `contracts/ConfidenceBreakdown.json`. Other repos link to these to stay aligned.

---

## Roadmap
**MVP (capstone)**
- Tool adapters: CNN, PSD/AIS, human reports; optional CLAP.  
- Fusion v1 (config weights), short-term dedupe, Postgres traces.  
- Observability v1 (logs/metrics), tiny dashboard in `orcasound-next`.

**Next**
- Moderator-feedback loop → adaptive thresholds/retraining hooks.  
- CLAP caption proposals + nearest-example evidence in alerts.  
- Monthly “acoustic health” summaries (LTSA/SPD roll-ups) for sponsors.  
- Multi-agent handoff (detector → communicator → insights).

---

## Why this framing helps (funders & partners)
- **Agentic AI** story that aligns with the course theme and current AI interest.  
- **Compliance & monitoring** value prop for ports, shipping, offshore energy.  
- **Open, modular architecture** that invites collaboration while preserving a clear integrator role.

---

## Governance & funding (context)
We’re exploring a **tiered access model** (free research tier → paid heavy research/commercial/compliance tiers) and **project-based partnerships** (e.g., baseline monitoring for EIS/EIA). See the accompanying **Governance & Funding Plan** (draft) for tiers, example APIs, case studies, and a 6–24 month action plan.

---

## Contributing
- Good first issues: fusion tweaks, trace UI, PSD/AIS adapters, report density calc, CLAP plugin.  
- Please keep payloads aligned with `contracts/` and add a tiny JSON example with any new field.  
- PRs welcome—be explicit about **assumptions** and **data sources** in the trace.

---

**Contact:** @adrmac • Orcasound contributors  
**License:** MIT/Apache-2.0 (TBD) • **Status:** Capstone MVP in progress
