# AI Compliance, Fraud Detection & Risk Analytics Platform

A full-stack compliance and transaction risk analytics platform combining an interactive Single-Page Application (SPA) dashboard, dynamic rule-building engine, cold database diagnostics, heuristic baseline models, and automated Scikit-Learn machine learning training pipelines.

---

## Architecture Overview

────────────────────────┐
                      │   Frontend Portal      │
                      │ (HTML5 / Vanilla JS)   │
                      └───────────┬────────────┘
                                  │ REST API / JSON
                                  ▼
                      ┌────────────────────────┐
                      │    Backend Server      │
                      │   (Flask / FastAPI)    │
                      └─────┬────────────┬─────┘
                            │            │
       ┌────────────────────┴──┐      ┌──┴────────────────────┐
       ▼                       ▼      ▼                       ▼
─────────────────────┐ ┌─────────────┐ ┌──────────────────┐ ┌─────────────────┐
│ SQLite Database     │ │ Rules Cache │ │ Pretrained Rules │ │ Trained ML      │
│ (project.db)      │ │ (/api/)   │ │ (.pkl models)  │ │ Random Forests  │
└─────────────────────┘ └─────────────┘ └──────────────────┘ └─────────────────┘

---

## Modules & File Structure

| Component / File | Language / Tech | Description |
| :--- | :--- | :--- |
| `index.html` (Auth) | HTML / CSS / JS | User authentication and account creation interface. |
| `home.html` (Dashboard) | HTML / CSS | High-level risk dashboard embedding timeline and proportion charts. |
| `config.html` (Config) | HTML / CSS / JS | Policy display card and system toggle controls (`data_pulling`, `rule_engine`). |
| `rules.html` & `rules_engine.js` | HTML / Vanilla JS | Interactive workspace for building, viewing, evaluating, and deleting dynamic rules. |
| `report.html` (Reports) | HTML / Vanilla JS | Audit log ledger and summary metrics view with date-range filters and CSV export. |
| `diagnostic.py` | Python / SQLite3 | Cold-record database schema inspector and row counter for `project.db`. |
| `generate_rules.py` | Python / Pickle | Compiles and serializes baseline heuristic rule models to `.pkl` binaries. |
| `train_pipeline.py` | Python / Pandas / Sklearn | Interactive multi-CSV training pipeline generating dataset-specific Random Forest models. |

---

## Detailed Component Breakdown

### 1. Frontend Web Interfaces & Logic

* **`Authentication (Login/Register)`:** Captures credentials and POSTs payloads to `/api/login` and `/api/register`, redirecting validated sessions to `/home`.
* **`Configuration Portal`:** Synchronizes real-time state switches with `/api/get-settings` and `/api/update-setting`.
* **`Dynamic Rule Matrix Engine (`rules_engine.js`)`:**
  * Maps transactional variables (`aod`, `narration`, `drcr`, `amount`, `cum_credit`, `cum_debit`, `channel`).
  * Manages client-side cache (`activeRulesCacheArray`) and performs dynamic DOM manipulation.
  * Formats active rule condition badges (`readonly-condition-badge`).
  * Validates and submits multi-clause rule objects to `/api/save-rule` or triggers deletions via `/api/delete-rule/<id>`.
* **`Audit Reporting & Analytics`:**
  * Uses `localStorage` to persist active calendar filter bounds across navigation.
  * Toggles between high-level rule summary matrices and row-level tabular audit ledgers (`/api/get-report-summary`, `/api/get-report-detailed`).

---

### 2. Backend Diagnostics & Heuristic Serialization

#### Database Inspector (`diagnostic.py`)
Checks table health, extracts schema definitions, and inspects raw tuples:
```python
# Verifies SQLite table structure
cursor.execute("PRAGMA table_info(transactions)")
actual_columns = [col[1] for col in cursor.fetchall()]

# Counts total processed records
cursor.execute("SELECT COUNT(*) FROM transactions")
total_rows = cursor.fetchone()[0]