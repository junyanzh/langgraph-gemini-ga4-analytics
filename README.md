# # TheLook E-commerce Analytics Agent with UQLM

A conversational analytics assistant for **TheLook** synthetic e-commerce data, now enhanced with **UQLM** (Uncertainty Quantification for Language Models) to measure confidence in each AI-generated response.

---
```plaintext
├── ga4/                                # Original GA4 PoC (unchanged on main)
│   ├── README.md
│   ├── ga4_schema.json
│   └── gemini_langgraph_ga4_query_agent.ipynb
│
└── thelook-uqlm/                       # This branch’s PoC for TheLook + UQLM
    ├── notebooks/
    │   └── thelook_uqlm_agent.ipynb    # Full Jupyter implementation
    ├── schema/
    │   └── thelook_schema.json         # Schema definition for TheLook dataset
    ├── diagrams/
    │   ├── workflow.mmd                # Mermaid workflow definition
    │   └── architecture.mmd            # Mermaid architecture overview
    ├── docs/
    │   └── tech_blog_outline.md        # Guidance for the Medium author
    ├── prompts/
    │   └── system_prompt.txt           # Updated system prompt for TheLook
    ├── tests/
    │   └── uqlm_tests.py               # UQLM scenario test scripts
    └── README.md                       # ← You are here
```

---

## ✨ What’s New in `thelook-uqlm`

1. **Dataset Swap**  
   - Switched from GA4 sample → `bigquery-public-data.thelook_ecommerce`  
   - New schema (`thelook_schema.json`) covers `orders`, `order_items`, `users`, `products`, `events`.

2. **UQLM Integration**  
   - Hybrid approach: try **real** UQLM BlackBoxUQ (semantic_negentropy + noncontradiction) in a separate thread; fallback to **intelligent simulation** if unavailable or times out.  
   - Expose `real_uqlm_scoring()` to generate multiple answer candidates and quantify their confidence scores.

3. **Extended Testing Suite**  
   - `tests/uqlm_tests.py` covers:  
     - **Normal queries** → expect high confidence (≥0.85)  
     - **No-data queries** → expect very high confidence (≈0.95)  
     - **Hallucination scenarios** → expect low confidence (≤0.6)  
     - **Threshold sensitivity analysis**  

4. **Interactive Agent**  
   - `notebooks/thelook_uqlm_agent.ipynb` with:  
     - Full LangGraph workflow  
     - `run_agent()` runner with UQLM scoring printed inline  
     - `interactive_chat()` + batch test functions  

5. **Documentation for Tech Writer**  
   - `docs/tech_blog_outline.md` contains the outline and figure list for the upcoming Medium post.

---

## 🚀 Quick Start

1. **Clone and switch branch** (via GitHub GUI or Desktop as described above)

2. **Install dependencies** (if running locally)
   ```bash
   pip install -U langchain_google_vertexai langgraph google-cloud-bigquery uqlm
   ```
## Configure your GCP project

In `notebooks/thelook_uqlm_agent.ipynb` Cell 2, set:

```python
PROJECT_ID = "your-gcp-project-id"
REGION     = "US"
```

---

## Run the Jupyter Notebook

Open `notebooks/thelook_uqlm_agent.ipynb`

Execute cells sequentially:

1. **Imports & installs**

2. **Schema definition**

3. **Tool & node functions**

4. **Workflow construction**

5. **UQLM integration (Cell 10)**

6. **Agent runner & tests**

---

## Try it out

In the notebook, run:

```python
run_agent("What is the total revenue from completed orders?")
```

UQLM candidate scores will print automatically.

---

## Launch interactive loop:
*(you can define and run this later in the notebook as needed)*

## Mermaid Diagrams (to embed)

**Workflow Diagram**  
`diagrams/workflow.mmd`

**System Architecture**  
`diagrams/architecture.mmd`

```plaintext
(Tech writer can export these with Mermaid Live Editor and include as figures in the blog.)
```

---

## 🔮 Future Work

- **Asynchronous UQLM in production**  
  Replace hybrid thread solution with true `asyncio` / FastAPI endpoint

- **Dynamic thresholding**  
  Adapt UQLM cutoff based on historical performance

- **Visualization layer**  
  Auto-chart candidate score distributions and final answers

- **Multi-model scoring**  
  Integrate white-box and adjudicator scorers alongside `BlackBoxUQ`

---

## 🤝 Contributing

This branch is exclusively for the **TheLook + UQLM PoC**. Feel free to:

- Report issues in this branch
- Submit PRs for:
  - Schema tweaks  
  - New UQLM scorers  
  - Richer test scenarios
- Review Mermaid diagrams (`*.mmd`) for clarity

```plaintext
Once finalized, we’ll merge into main or tag a v1.0-uqlm release.
```

