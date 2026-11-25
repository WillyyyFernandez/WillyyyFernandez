# William Fernández

Data engineering student in Mérida, México (Universidad Politécnica de Yucatán, Data & AI Engineering).

I like pipelines where you can tell what happened to every row: raw data that is never overwritten, tests on the numbers, a freshness check that fails loudly, and infrastructure you can rebuild from the repo. I also work with small local language models, mostly keeping them on a short leash.

Open to data engineering opportunities. [Email](mailto:fernandezcastillowilliam@gmail.com) or [LinkedIn](https://www.linkedin.com/in/william-fernández).

## Projects

### [Monitor financiero](https://github.com/WillyyyFernandez/monitor-financiero) ([live dashboard](https://monitor-financiero-4seqidqnjmqmloijab7px7.streamlit.app/))
Daily pipeline for Mexican financial data: exchange rate, TIIE, CETES and inflation from Banxico, plus BTC, ETH and USDT from Binance.

- Every download is written as a new Parquet file and never rewritten. Staging keeps the latest version of each value, so a Banxico revision replaces the old number without losing what was published before.
- dbt on DuckDB, orchestrated with Dagster on a daily schedule. 43 dbt tests cover uniqueness, nulls, plausible ranges and data freshness. A second job checks load freshness and goes red if the daily run did not happen.
- CI runs the whole dbt build on a fixed sample of public data, so it needs no network and no tokens.
- Natural-language agent on a local Qwen2.5-14B. The model never writes SQL: it picks one of six functions and the code runs a parameterized, read-only query. Scored 6/6 on a small question set, including one with no data where it correctly declined.

`Python` `dbt` `DuckDB` `Dagster` `Docker` `GitHub Actions` `Streamlit`

### [PulseCore DC Dashboard](https://github.com/WillyyyFernandez/pulsecore-dc-dashboard)
Operations dashboard for a fictional Tier III data center, built to practice the deployment side: containerized Streamlit app, with the VPC, ECS cluster and load balancer defined in Terraform and a GitHub Actions build. The data is illustrative; the point is that the environment can be recreated from the repo.

`Python` `Streamlit` `Terraform` `AWS ECS` `Docker` `Plotly`

### [COVID-19 Mortality Analysis](https://github.com/WillyyyFernandez/covid19-mortality-analysis)
ETL and analysis of 137,030 records from Mexico's official surveillance data, feeding a three-page Power BI dashboard (case fatality rate by state, age and comorbidity). I kept the 1.05M-row Kaggle historical dataset out of the main fact table on purpose: merged, the hospitalization KPIs counted rows with no status while the case count respected the filters, and the two disagreed silently. The notebook documents that decision, the sentinel codes (97, 98, 99) treated as missing, and the limits of the CFR estimate.

`Python` `Pandas` `Plotly` `Power BI`

### Team projects

**[ARC-AGI Failure Modes & Evaluation Pipeline](https://github.com/WillyyyFernandez/atlas-agi)** (5 people). Pipeline to study how local and frontier LLMs fail on the ARC-AGI benchmark: ETL, batch inference, scoring, a failure taxonomy and analytics tables in Parquet. I owned the data layer:
- ETL that reads every task JSON of the evaluation split and writes a clean table to Parquet, partitioned by split and task ID.
- Quality validation that catches empty grids, inconsistent dimensions and null fields, and writes the rejects to a separate log and error table.
- The Parquet schema (15 columns, with a hash per grid), documented type decisions, and a schema freeze so the inference and analytics work could build on it without surprises.
- Built to rebuild identically: delete the output, rerun, get the same table.

`Python` `Pandas` `PyArrow` `DuckDB`

**[TUP-detection](https://github.com/notyorch/TUP-detection)** (4 authors, Apart Research Global South Hackathon 2026). Prompt-injection detection engine: deterministic regex rules first, then a neural classifier (Sentinel v2) for what the rules miss, with an optional LLM judge for the gray zone. 95.1% balanced accuracy on the deepset PINT split (n = 662). Team project; I worked on the detection pipeline.

**[Climavis](https://github.com/WillyyyFernandez/climavis-full-stack)** (team project). Interactive climate dashboard for Mexican states: React and TypeScript front end with a Leaflet map, and a FastAPI back end over a SQLite database of historical observations, with endpoints for states, daily records and per-state statistics.

`Python` `FastAPI` `SQLAlchemy` `SQLite` `React`

## Tools I use

- **Pipelines and storage:** Python, SQL, dbt, DuckDB, Dagster, Parquet
- **Analytics:** Pandas, Power BI, Streamlit
- **Backend:** FastAPI, SQLAlchemy
- **Infra:** Docker, Terraform, AWS (ECS), GitHub Actions
- **Local LLMs:** llama.cpp, function calling, JSON schema validation
