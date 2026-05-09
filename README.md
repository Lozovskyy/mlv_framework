# Fabric MLV Deployment Wrapper

This repository contains a PySpark framework for managing the lifecycle of Materialized Lake Views (MLVs) in Microsoft Fabric. 

Native MLV deployments in Fabric lack built-in state tracking for DDL changes and are limited to time-based scheduling. This wrapper solves that by making deployments fully idempotent. It uses MD5 hashing to track query logic, storage strategies (partitioning/clustering), and constraints, storing the state directly inside Delta `TBLPROPERTIES`.

**Key Features:**
* **Idempotency:** Automatically routes between `CREATE OR REPLACE`, `DROP/CREATE`, and `REFRESH` based on detected drift in the underlying SQL logic or metadata.
* **Event-Driven Orchestration:** Bypasses native time-bound schedulers, allowing MLV refreshes to be triggered directly from Fabric Pipelines based on upstream data ingestion.
* **Concurrency:** Designed to be executed in parallel using Python `ThreadPoolExecutor`.
* **Built-in Observability:** Leverages Fabric's native `sys_dq_metrics` for monitoring data quality and dropped rows without custom logging overhead.
