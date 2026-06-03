# 🇪🇺 Eurostat — Portolan catalog

A Portolan spatial-data catalog from **Eurostat**: git-sourced metadata, published to object storage as a **static Apache Iceberg REST catalog** + STAC + OGC API - Records + direct download — readable with no server.

**Catalog endpoint (Iceberg REST):** `https://8et4c.upcloudobjects.com/carto-ogc-connect-helsinki/repo/portolan-eurostat-catalog`

## Datasets

| Dataset | What it is | Access |
|---|---|---|
| **Industrial electricity price** (`electricity_prices`) | Eurostat industrial electricity prices by country, including taxes and levies, in EUR per kWh. One row per country (ISO geo code + name) and reporting semester (e.g. 2025-S2), with the EU-27 aggregate included for benchmarking. Useful for comparing Finland's energy cost against EU peers. Non-geospatial table — country-level, no geometry. | `tab.eurostat_elec` |

## Read it — no credentials, no server

**ATTACH (DuckDB / Snowflake):**
```sql
INSTALL iceberg; LOAD iceberg; INSTALL httpfs; LOAD httpfs;
ATTACH 'cat' (TYPE iceberg, ENDPOINT 'https://8et4c.upcloudobjects.com/carto-ogc-connect-helsinki/repo/portolan-eurostat-catalog', AUTHORIZATION_TYPE 'none');
SELECT * FROM cat.tab.eurostat_elec LIMIT 10;
```

**Discover:** [`catalog.json`](catalog.json) (STAC) · [`records/catalog.json`](records/catalog.json) (OGC API - Records) · [`index.html`](https://8et4c.upcloudobjects.com/carto-ogc-connect-helsinki/repo/portolan-eurostat-catalog/index.html) (human view). Direct GeoParquet download links are in the explorer.

## Contributing
Fix or extend the catalog with a **pull request** (edit `portolan.config.json` / `datasets/<id>.json` / the Iceberg metadata, then run `tools/generate_stac.py` + `tools/validate.py`); a merge republishes to the bucket. Data bytes live on the bucket, never in git. See [`AGENTS.md`](AGENTS.md) for the agent-facing guide and the opt-in usage-report channel.

## License
Data: **Eurostat — free reuse with attribution** (© Eurostat (EU statistical office)). Tooling: Apache-2.0. See [`LICENSE`](LICENSE).

## Part of a federation
One child of the Portolan Helsinki *catalog of catalogs*: `https://8et4c.upcloudobjects.com/carto-ogc-connect-helsinki/catalog/stac.json`

