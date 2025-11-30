**Problem Statement**

In India, over 300 million vehicles face maintenance challenges: scarce expert mechanics (especially rural), diagnostic delays costing 20-30% extra in repairs, opaque part pricing across OEM/aftermarket, and no instant reporting for customers. Car owners wait days; garages lose revenue. This is critical—vehicle downtime affects livelihoods (e.g., taxi drivers), safety (undiagnosed issues cause 15% accidents), and economy (₹2 lakh crore auto aftermarket). Mechanic-Mitra solves this with instant AI diagnostics from photos/audio, slashing time from hours to seconds, providing precise part recommendations with INR prices, and pro reports—empowering garages for faster service, accurate quotes, trust-building.

**Why agents?**

Agents excel for complex, multi-step workflows needing specialization + collaboration. Single LLMs hallucinate prices/details; agents decompose: DiagnosticAgent orchestrates analysis (vision/audio → diagnosis), delegates to PriceAgent for accurate market estimates via tool-calling. This mirrors enterprise teams (lead mechanic + pricer), ensures reliability (tools > generation), efficiency (1 API call vs. 3), and extensibility (add agents for scheduling/parts ordering). Tool-calling handles dynamic data (prices), retries manage unreliability—perfect for production business tools where single-model limits fail.

**What I created**

Mechanic-Mitra: Multi-agent diagnostic platform.

**Architecture:**

- **PriceAgent**(Gemini 2.5 Flash): Estimates realistic INR ranges (e.g., "INR 2500-4500") for parts, context-aware (vehicle/year).

- **DiagnosticAgent** (Gemini 2.5 Flash + tools): Multimodal analysis (image: rust/leaks; audio: knocking) → JSON (analysis, components, parts via tool).

- **PDF Generator**: Pro reports with tables (components/conditions, priced parts + totals).

- **UI**: ipywidgets for uploads/previews.

- **Flow**: Upload → Single chat (image/audio/prompt) → Tool-called prices → Parsed JSON → HTML/PDF output. Optimized: 67% quota save, retries/backoff.


**The Build**

  - Core: Google Gemini 2.5 Flash (multimodal, tools).

  - Agents: Custom classes; tool integration (get_market_prices).

  - UI/IO: ipywidgets, PIL/Audio previews, tempfile for uploads.

  - Output: FPDF2 custom class (headers, tables w/ FontFace, totals).

  - Production: Kaggle Secrets/.env, JSON safe-parse, 3x retries (exp backoff), fallbacks.

  - Libs: google-generativeai, fpdf2, Pillow, ipywidgets.

  - Built iteratively: Prompt engineering for JSON/tools, PDF debugging, quota tests. Deploy-ready (Streamlit next).

**If I had more time, this is what I'd do**

  - Real-time scraping (e.g., IndiaMart APIs) for live prices.
  - Multi-lang (Hindi via translation agent).
  - OBD-II/AR integration (live data, mobile camera).
  - CrewAI/LangGraph for 5+ agents (scheduling, inventory).
  - Benchmarks: 1000 diagnostics dataset.
  - Streamlit/Cloud Run deployment w/ auth.
  - Fine-tune Gemini on auto datasets.[](url)
