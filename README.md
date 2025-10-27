# DS-2002-Kenny-Data-Project-1

## Documentation — Process, Code, and Deployment Strategy

### 1) Overview
This project builds a **data mart** for a fictional transit system. Data is extracted from CSV and MongoDB, **staged** in MySQL, then **loaded** into dimensional tables and a fact table for analysis (riders by route, stop, and time).

**Fact:** `fact_riders`  
**Dimensions:** `dim_date`, `dim_routes`, `dim_buses`, `dim_stops`  

### 2) Data Sources
- **CSV (file system)**
  - `routes_catalog*.csv`: route data 
  - `stops*.csv`: stop data 
  - `daily_riders*.csv`: riders fact
- **MongoDB (NoSQL)**
  - Collection: `buses` 
- **MySQL Workbench**
  - `dim_date` 

### 3) Schema Design
- `fact_riders`
- `dim_date`
- `dim_routes`
- `dim_buses`
- `dim_stops`

`stg_*` tables are a temporary store to hold raw data. `dim_*` tables add surrogate keys and attributes for the analytical queries. The fact links to all dims through surrogates.

### 4) ETL Process 

**1. Create / Reset DB (Notebook)**
- Drop & create database: `transit_dw`
- Create tables by running datable creation cells

**2. Populate Date Dimension (Workbench)**
- Run `dim_date_create_transit.sql`

**3. Stage Source Data**
- Run Staging : `set_dataframe()` → `stg_routes`, `stg_stops`, `stg_riders`; Mongo → `stg_buses`

**4. Load Dimensions (Notebook)**
- `TRUNCATE` then `INSERT ... SELECT` from staging into `dim_routes`, `dim_buses`, `dim_stops`
- Column count is then modified (adds surrogate keys)

**5. Load Fact + Integrate Keys (Notebook)**
- `TRUNCATE fact_riders`
- `INSERT ... SELECT` from `stg_riders` 
- Update surrogate keys: `date_key`, `route_key`, `bus_key`, `stop_key`

**Steps**
1. Open the notebook in VS Code.
2. Run the Drop/Create DB cell → then Create tables cells.
3. In MySQL Workbench, run `dim_date_create_transit.sql`.
4. Load staging
5. Run dimension load cell.
6. Run fact load + key integration cell.
7. Run validation cells (row counts, missing keys).
8. Run analysis queries (top routes, avg fare by month, busiest stops, route*month).

