Apache Solr Benchmark Workloads
--------------------------------

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

This repository contains the default workload specifications for
[Apache Solr Benchmark](https://github.com/janhoy/solr-benchmark),
the macrobenchmarking framework for [Apache Solr](https://solr.apache.org/).

You do not need to interact with this repository directly unless you want to inspect existing
workloads, run benchmarks with a custom workload, or contribute a new workload.

## How to contribute a change

See an area to make improvements or add support? Follow these steps:

1. Fork this repository and create a feature branch from `main`.
2. Make your change and test it locally against a running Solr cluster using `solr-benchmark`
   in `--test-mode`.
3. Open a pull request against `main` of this repository, describing what you changed and how
   you tested it.

See [CONTRIBUTING.md](CONTRIBUTING.md) for full details.

## How to contribute a workload

See [USER_GUIDE.md](USER_GUIDE.md) for the structure of a workload and the shared
`common_operations/` library.

## Getting help

- Questions and discussion: [dev@solr.apache.org](https://lists.apache.org/list.html?dev@solr.apache.org)
- Bug reports and feature requests: [GitHub Issues](https://github.com/janhoy/solr-benchmark-workloads/issues)
- Benchmark tool documentation: [solr-benchmark README](https://github.com/janhoy/solr-benchmark/blob/main/README.md)

## License

This repository is licensed under the [Apache License 2.0](LICENSE).
Individual workloads may incorporate datasets with additional licensing terms;
see each workload's `README.md` for details.
