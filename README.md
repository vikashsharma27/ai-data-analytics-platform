# 📊 AI-Powered Automated Data Analytics & Dashboard Platform

An end-to-end Streamlit application that turns any CSV or Excel file into a
fully interactive, adaptive BI dashboard — automatically. Upload a dataset
and the platform understands it, cleans it, detects KPIs, builds charts,
writes insights, tells a data story, and lets you ask it questions in plain
English.

```
Upload → Understand → Clean → Analyze → KPIs → Charts → Dashboard →
Insights → Data Story → Recommendations → Ask Your Data → Reports
```

---

## ✨ Features

- **Universal file upload** — CSV, TSV, XLSX, XLS, including multi-sheet
  Excel workbooks, with a friendly error for corrupted/empty/unsupported files.
- **Automatic data understanding** — intelligently classifies every column
  as numeric, categorical, date, or ID, and detects the *business meaning*
  of columns (revenue, profit, cost, quantity, discount, customer, product,
  category, region, geography...) using keyword + statistical heuristics
  rather than hardcoded column names.
- **Automated data cleaning** — detects duplicates, missing values, invalid
  dates, outliers, empty/constant columns, and high-cardinality columns;
  cleans a **copy** of the data (never mutates the original) and shows an
  exact change log.
- **Automatic KPI detection** — Total Revenue, Total Profit, Profit Margin,
  Total Quantity, Average Order Value, Number of Customers/Products/Orders,
  Average Discount, and more — only shown when the underlying columns exist.
- **Adaptive dashboard** — the dashboard, its charts, and even the tab list
  change based on what's actually in your dataset. No customer column? No
  Customer Analysis tab.
- **Intelligent chart selection** — line/area for time+numeric, bar for
  category+numeric, scatter for two numerics, histogram/box for
  distributions, donut for part-to-whole, choropleth for geography,
  heatmap for correlation.
- **Automated insight generation** — every insight sentence is built from
  real calculated values (never fabricated).
- **Data Story module** — a 6-part narrative (What happened / Why / Where /
  What changed / What's concerning / What should we do) written like a
  business analyst's executive summary.
- **Ask Your Data** — a natural-language Q&A box. All numbers are computed
  with pandas first; an optional LLM (Anthropic or OpenAI) can be plugged
  in purely to phrase the answer more naturally — it never invents numbers.
- **Advanced analytics** — correlation analysis, IQR & Z-score outlier
  detection, Pareto (80/20) analysis, RFM customer segmentation, and a
  simple linear-trend forecast (clearly labelled as a prediction).
- **Data Quality Score (0–100)** with a full breakdown of what drove the score.
- **Downloadable reports** — cleaned dataset (CSV/Excel), filtered dashboard
  data (CSV), a full HTML analysis report, and a plain-text insights report.
- **Dynamic sidebar filters** — date range and any detected category/region
  columns, all wired directly into every chart and KPI.
- **Handles large files** — Streamlit caching, efficient pandas operations,
  and a codebase built to comfortably work with 100,000+ row datasets.

---

## 🧱 Technology Stack

| Layer | Technology |
|---|---|
| Web framework | Streamlit |
| Data processing | Pandas, NumPy |
| Statistics / ML | Scikit-learn, SciPy, Statsmodels |
| Visualization | Plotly (primary), Matplotlib, Seaborn |
| Excel/CSV I/O | Pandas, OpenPyXL |
| Optional AI | Anthropic Claude or OpenAI (natural-language phrasing only) |

---

## 📂 Folder Structure

```
project/
├── app.py                     # Main Streamlit application (UI + orchestration)
├── requirements.txt
├── README.md
├── .env.example                # Template for optional LLM API keys
├── data/
│   ├── generate_sample_data.py # Generates the demo dataset
│   └── sample_sales_data.csv   # 1,200+ row realistic demo dataset
├── modules/
│   ├── data_loader.py          # CSV/Excel upload & parsing, friendly errors
│   ├── data_cleaner.py         # Automated cleaning (works on a copy)
│   ├── data_profiler.py        # Dataset & per-column profiling
│   ├── kpi_engine.py           # Automatic KPI detection & calculation
│   ├── insight_engine.py       # Text insights derived from real values
│   ├── chart_engine.py         # Intelligent Plotly chart selection
│   ├── storytelling.py         # 6-part "Data Story" narrative builder
│   ├── statistics.py           # Descriptive stats, outliers, correlation,
│   │                            # Pareto analysis, RFM segmentation
│   ├── forecasting.py          # Moving average & linear-trend forecast
│   ├── ask_data.py             # Natural-language "Ask Your Data" engine
│   ├── llm_client.py           # Optional LLM wrapper (phrasing only)
│   ├── quality_score.py        # 0–100 Data Quality Score
│   └── report_generator.py     # HTML/TXT report + CSV/Excel export builders
├── utils/
│   ├── helpers.py               # Number/percent formatting, safe division
│   └── column_detection.py      # Structural + semantic column detection
└── assets/                      # (reserved for custom static assets)
```

Nothing lives directly in `app.py` beyond page wiring and layout — every
analytical capability is a reusable, independently testable module.

---

## 🚀 Installation

1. **Clone / copy this project folder**, then move into it:
   ```bash
   cd project
   ```

2. **Create a virtual environment (recommended):**
   ```bash
   python -m venv venv
   source venv/bin/activate      # Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **(Optional) enable AI-enhanced phrasing:**
   ```bash
   cp .env.example .env
   # then edit .env and add ANTHROPIC_API_KEY or OPENAI_API_KEY
   ```
   The app works fully without this step — this only affects natural-language
   phrasing quality, never the numbers themselves.

---

## ▶️ How to Run

```bash
streamlit run app.py
```

Then open the URL Streamlit prints (typically `http://localhost:8501`).

---

## 📤 How to Upload Data

1. Use the sidebar **"Upload CSV or Excel file"** control, or click
   **"Use sample sales dataset"** to try the platform instantly with the
   bundled demo data.
2. If you upload a multi-sheet Excel workbook, pick which sheet to analyze
   from the dropdown that appears.
3. Adjust cleaning options (duplicate removal, missing-value fill strategy,
   date conversion) in the sidebar — the dashboard updates immediately.
4. Use the dynamic filters (date range, region, category, product, ...) to
   slice the dashboard; every KPI and chart responds to your selection.

---

## 🧠 How the Analytical Engine Works

1. **`data_loader`** reads the file safely and returns one DataFrame per sheet.
2. **`column_detection`** classifies every column structurally (numeric /
   categorical / date / id) and semantically (revenue, profit, customer,
   region, ...) using column-name keyword matching combined with dtype and
   uniqueness-ratio heuristics — so it generalizes beyond any specific
   dataset's exact column names.
3. **`data_cleaner`** detects issues and produces a cleaned *copy*, with a
   full change log so nothing happens silently.
4. **`kpi_engine`** turns detected roles into concrete KPI values, only
   surfacing KPIs the data actually supports.
5. **`chart_engine`** picks the right chart type for each combination of
   detected roles and renders it with Plotly.
6. **`insight_engine`** and **`storytelling`** turn calculated numbers into
   plain-English sentences — every claim traces back to a pandas calculation.
7. **`ask_data`** matches your question to a calculation pattern (top-N,
   which-region-highest, margin, trend, totals, etc.) and computes the
   answer live; `llm_client` can optionally rephrase that computed answer.
8. **`report_generator`** assembles everything into downloadable HTML/TXT
   reports and CSV/Excel exports.

---

## 🖼️ Example Dashboard Output

When you load the bundled `sample_sales_data.csv`, you'll see a dashboard
titled **"Sales Performance Analytics"** with KPI cards for Total Revenue,
Total Profit, Profit Margin, Total Orders, Number of Customers, and Average
Order Value; a monthly revenue trend chart; a Top-10-by-Category bar chart;
a revenue-share donut chart by category; and a Region vs Product scatter/
correlation view — plus full Customer, Product, Regional, Statistical,
Insights, Data Story, Ask Your Data, and Reports tabs.

---

## 🔒 Security Notes

- Uploaded files are processed in memory for the session only and are not
  permanently written to disk.
- Uploaded files are never executed as code.
- If you enable the optional LLM integration, keep your API key in `.env`
  (see `.env.example`) — never hardcode it in source.

---

## 🧭 Future Improvements

- Persist analysis sessions so users can return to a previously uploaded dataset.
- Add cohort analysis and K-Means-based customer clustering as a dedicated tab.
- Support direct database connections (Postgres/MySQL/BigQuery) as a data source.
- Add PDF export of the analysis report (currently HTML/TXT).
- Add user authentication and saved-dashboard sharing for team use.
- Swap the linear-trend forecast for a Statsmodels ETS/ARIMA model when
  enough history is available.

---

## 🧪 Regenerating the Sample Dataset

```bash
python data/generate_sample_data.py
```

This produces a fresh `data/sample_sales_data.csv` with 1,200+ realistic
records (Order_ID, Order_Date, Customer_ID/Name/Segment, Product, Category,
Region, City, Quantity, Unit_Price, Discount, Sales, Cost, Profit) —
including a few intentionally realistic duplicates, missing values, and
outliers so the cleaning and quality-scoring features have something to do.
