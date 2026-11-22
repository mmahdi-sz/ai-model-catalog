---
name: model-catalog
description: Query the private AI model catalog to compare benchmarks, find exact model variants and weight files, and assess local deployment requirements. Use when given the repository, its catalog ZIP, or its extracted CSV tables.
---

# Use the private model catalog

## Open the data

Use the archive supplied by the user. If given the repository, follow the current ZIP link in its README. If given extracted CSVs, read them directly. Read ZIP members in place or extract them to a temporary directory. Use a CSV parser and UTF-8 with optional BOM support. Inspect the actual column headers before joining files.

## Find and compare models

1. Start with the CSV whose filename begins with `02_Master`. Filter using the requested modality, model, precision, and requirements. Keep distinct release and evaluation configurations separate.
2. Use `Primary_Benchmark_Label` to interpret `Primary_Score_Numeric`. Compare results only when the metric, methodology version, and direction agree. `Base_Intelligence_Score` belongs to the parent model. Use `Variant_Measured_Score` for a measured result on the exact variant. Preserve blanks as missing.
3. Search `06_Legacy_Model_Review.csv` for original model records. Use the row's `Catalog_ID` as `Input_ID` in `05_Source_Row_Map.csv` to retrieve its decision and any active destination. Join benchmark history from `08_Benchmark_History.csv` using `Input_ID`.

## Verify download and deployment evidence

- Match `Catalog_ID` to `07_Weight_Manifest.csv` and return the pinned revision and all relevant file URLs. Distinguish a repository landing page, a listed weight file, and evidence of a complete checkpoint. Report uncertainty for missing manifests, unresolved shards, or a precision mismatch.
- Use the hardware and KV reference tables beginning with `01` and `03`, plus the enrichment evidence in `04`. Keep weight size, publisher capacity, minimum VRAM, and measured consumption separate. State the engine, precision, context, batch, and GPU configuration when documented.
- Consult `09_Repository_Checks.csv` and its evidence origin when judging freshness. Treat a rate-limited query as unresolved. A repository response establishes reachability or metadata availability. Claims about successful inference require execution evidence.

## Report the result

Give the exact model or variant, relevant benchmark and date, download evidence, deployment evidence, and unresolved fields. Cite the CSV names and row IDs or original URLs so the user can reproduce the lookup. Keep this catalog and its Artificial Analysis data in the private workflow. For repository maintenance, consult `AGENTS.md` when available.
