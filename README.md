# NYC taxi dbt project

[![dbt build & publish](https://github.com/Nuri1977/nyc-taxi-dbt/actions/workflows/dbt_build.yml/badge.svg)](https://github.com/Nuri1977/nyc-taxi-dbt/actions/workflows/dbt_build.yml)
[![docs](https://img.shields.io/badge/docs-live-ff7a1a)](https://nuri1977.github.io/nyc-taxi-dbt/)

A dbt + DuckDB project on the NYC TLC green taxi trips of 2025, built in Rosetta DBT Studio.
GitHub Actions builds and tests it on every push, publishes the warehouse, and deploys the docs.

| What | How | Where to look |
|---|---|---|
| Pipeline | raw table → `trips` view → three mart tables | `models/` |
| Tests | unique, not_null, accepted_values | `models/schema.yml`, `seeds/seeds.yml` |
| Docs | dbt docs, lineage graph | [docs site](https://nuri1977.github.io/nyc-taxi-dbt/) |
| CI | `dbt build` on every push and every Monday | `.github/workflows/dbt_build.yml` |
| Environments | dev in Studio, prod in CI | `profiles.yml`, `ci/profiles.yml` |
| Warehouse | `prod.duckdb`, a release asset | [Releases](https://github.com/Nuri1977/nyc-taxi-dbt/releases/tag/warehouse) |
| Data | 12 Parquet files and the dev database, in the repo | `data/raw/`, `nyc_taxi_dev.duckdb` |

## Query the warehouse

In the DuckDB CLI or any DuckDB client:

```sql
install httpfs; load httpfs;
attach 'https://github.com/Nuri1977/nyc-taxi-dbt/releases/download/warehouse/prod.duckdb' as taxi (read_only);
select * from taxi.main.top_pickup_zones;
```

The file holds the raw trips, the `trips` view and the three mart tables.

## Run it yourself

Clone the repository and open the folder in [Rosetta DBT Studio](https://rosettadb.io/download-dbtstudio.html).
The raw files and the dev database are included.

Or from a terminal, the way CI does it:

```bash
pip install -r requirements.txt
export DUCKDB_PATH=data/prod.duckdb
python scripts/download_data.py
dbt build --profiles-dir ci
```

## Data

NYC Taxi & Limousine Commission trip records: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

Course: dbt + DuckDB in Rosetta DBT Studio. One commit per lesson.
