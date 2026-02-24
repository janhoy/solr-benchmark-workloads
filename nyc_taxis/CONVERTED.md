# Workload Conversion Record

This workload was converted from OpenSearch Benchmark format to Solr Benchmark format.

## Notes

- Search operation bodies have been translated to Solr JSON Query DSL format.
- Configsets were auto-generated from OpenSearch mappings (review for production use).
- Corpora (dataset files) are unchanged and shared with the source workload.
- Test procedures and operations with no Solr equivalent were skipped