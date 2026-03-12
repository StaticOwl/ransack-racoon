# ADR-012 — DataSource Pushdown Strategy
**Status:** ✅ Accepted
**Decision:** Trait-based pluggable source abstraction with optional manual pushdown
**Design:**
```
DataSource (trait)
├── supportsQueryPushdown: Boolean
├── pushdownQuery(sql: String): DataFrame   // warehouse sources only
└── read(config: SourceConfig): DataFrame   // all sources
```
**Rule:** Manual pushdown only for warehouse sources (BigQuery, Snowflake, Redshift). File sources use Spark automatic pushdown.
**Watch:** Pushdown SQL must be source-dialect aware.
