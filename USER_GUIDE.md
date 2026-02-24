# Apache Solr Benchmark Workloads — User Guide

[Apache Solr Benchmark](https://github.com/janhoy/solr-benchmark) (`solr-benchmark`) is a
macrobenchmarking framework for [Apache Solr](https://solr.apache.org/). A *workload* tells
`solr-benchmark` everything it needs to run a benchmark: which Solr collections to create, which
documents to index, and which search and faceting operations to execute.

This guide explains how this repository is structured and how to use and create workloads.

### Contents

- [Repository layout](#repository-layout)
- [Common operations](#common-operations)
- [Running a workload](#running-a-workload)
- [Selecting a test procedure](#selecting-a-test-procedure)
- [Workload parameters](#workload-parameters)
- [Branches and versioning](#branches-and-versioning)
- [Contributing a workload](#contributing-a-workload)


## Repository layout

```
common_operations/          # Reusable schedule snippets shared across all workloads
<workload-name>/
  workload.json             # Top-level workload definition (collections, corpora, operations, test_procedures)
  workload.py               # Optional Python parameter sources
  operations/               # Named operation definitions (JSON)
  test_procedures/          # Benchmark schedules that reference the operations above
  configsets/<name>/        # Solr configset files uploaded when creating the collection
    schema.xml
    solrconfig.xml
    …
  files.txt                 # Lists the corpus data files for this workload
  README.md                 # Workload-specific documentation and parameter reference
```


## Common operations

The top-level `common_operations/` directory contains reusable JSON schedule snippets that can be
included in any workload's test procedures via the Jinja2 `benchmark.collect` helper:

```jinja
{{ benchmark.collect(parts="../../common_operations/create_collection.json") }}
```

Paths in `benchmark.collect` are resolved relative to the template file that contains the call.
From a test procedure file at `<workload>/test_procedures/foo.json` the common operations are two
levels up (`../../common_operations/`).

### Available snippets

| File | Operation type | Template variables |
|------|---------------|--------------------|
| `create_collection.json` | `create-collection` | `collection_name` *(required)*, `configset_path` *(default: `collection_name`)*, `num_shards` *(default: 1)*, `replication_factor` *(default: 1)* |
| `delete_collection.json` | `delete-collection` | `collection_name` *(required)* |
| `optimize.json` | `optimize` | `max_segments` *(default: 1)*, `optimize_timeout` *(default: 600 s)* |
| `check_cluster_health.json` | `raw-request` (CLUSTERSTATUS) | *(none)* |

Pass template variables via a `{% with %}` block, for example:

```jinja
{% with collection_name="nyc_taxis", configset_path="configsets/nyc_taxis" %}
{{ benchmark.collect(parts="../../common_operations/create_collection.json") }},
{% endwith %}
```

`delete_collection.json` always sets `ignore-missing: true` and `delete-configset: true`, so it
is safe to call unconditionally at the start of a test procedure.

`check_cluster_health.json` calls the Solr Collections API (`CLUSTERSTATUS`) to verify the
cluster is reachable. It does not require any parameters and does not block until a specific
health state is reached — use it as a quick connectivity check after collection creation.


## Running a workload

### Against an existing Solr cluster (benchmark-only pipeline)

```bash
solr-benchmark run \
  --pipeline=benchmark-only \
  --target-host=localhost:8983 \
  --workload=nyc_taxis \
  --test-mode
```

`--test-mode` drastically reduces corpus size and iteration counts for a quick sanity check.

### Provision Solr via Docker, then benchmark

```bash
solr-benchmark run \
  --pipeline=docker \
  --distribution-version=9.10.1 \
  --workload=nyc_taxis \
  --test-mode
```

### Using a local workload checkout

```bash
solr-benchmark run \
  --pipeline=benchmark-only \
  --target-host=localhost:8983 \
  --workload-path=/path/to/solr-benchmark-workloads/nyc_taxis
```

### Pointing at a forked workloads repository

```bash
solr-benchmark run \
  --workloads-repository=https://github.com/<YOUR USERNAME>/solr-benchmark-workloads \
  --workload=nyc_taxis \
  --test-mode
```


## Selecting a test procedure

Each workload ships with one or more *test procedures* — named benchmark schedules defined in
the `test_procedures/` directory. Use `--test-procedure` to select one:

```bash
solr-benchmark run \
  --workload=nyc_taxis \
  --test-procedure=append-no-conflicts-index-only
```

The default test procedure (marked `"default": true` in the workload) is used when
`--test-procedure` is not specified.

Available test procedures for a workload are documented in that workload's `README.md`.


## Workload parameters

Most workloads expose Jinja2 template parameters that let you tune the benchmark without
modifying any files. Pass them with `--workload-params`:

```bash
solr-benchmark run \
  --workload=nyc_taxis \
  --workload-params="bulk_indexing_clients:4,num_shards:2,replication_factor:2"
```

Available parameters are documented in each workload's `README.md`.

Common parameters shared across workloads:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `bulk_indexing_clients` | 8 | Number of parallel indexing clients |
| `bulk_size` | 10000 | Documents per indexing batch |
| `num_shards` | 1 | Shards for the Solr collection |
| `replication_factor` | 1 | Replication factor for the Solr collection |
| `max_segments` | 1 | Target segment count for the optimize step |
| `optimize_timeout` | 600 | Timeout in seconds for the optimize operation |
| `target_throughput` | varies | Max requests per second for search operations (`none` for unlimited) |
| `search_clients` | 1 | Number of parallel search clients |
| `error_level` | `non-fatal` | Error handling for bulk operations |


## Branches and versioning

Workloads currently live on the `main` branch.

If Solr-version-specific variations are needed in the future, version branches will be created
following the naming convention `solr-9`, `solr-10`, etc. `solr-benchmark` will automatically
select the branch that matches the major version of the target Solr cluster, falling back to
`main` if no matching branch exists.


## Contributing a workload

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full contribution process, including how to test
your workload locally and how to open a pull request.

A new workload should at minimum provide:

- `workload.json` — defining `collections`, `corpora`, `operations`, and `test_procedures`
- `configsets/<name>/` — a valid Solr configset (`schema.xml` + `solrconfig.xml`)
- `operations/default.json` — the named operations referenced by test procedures
- `test_procedures/default.json` — at least one test procedure (mark one `"default": true`)
- `README.md` — description, example document, and parameter reference
- `files.txt` — list of corpus data files

Reuse the shared `common_operations/` snippets for collection lifecycle and optimize steps
rather than duplicating those definitions inside each workload.
