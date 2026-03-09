# Workload Conversion Record

This workload was automatically converted from OpenSearch Benchmark format to
Solr Benchmark format by `osbenchmark.solr.conversion.workload_converter`.

## Metadata

- **Source workload**: `/Users/janhoy/git/opensearch-benchmark-workloads/geonames`
- **Converted at**: `2026-03-09T01:43:30Z`
- **Converter version**: solr.conversion.workload_converter v1.0

## Skipped Operations

- `cluster-health`

## Conversion Issues

- test_procedures/default.json: cannot parse as JSON fragment (Cannot parse as JSON after Jinja2 substitution: Expecting ',' delimiter: line 7 column 9 (char 459)); copied verbatim

## Notes

- Search operation bodies have been translated to Solr JSON Query DSL format.
- Configsets were auto-generated from OpenSearch mappings (review for production use).
- Corpora (dataset files) are unchanged and shared with the source workload.
- Operations with no Solr equivalent were skipped (listed above if any).

Re-running `convert-workload` with `--force` will overwrite this directory.
