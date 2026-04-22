# ASF Incubation Checklist

This checklist tracks tasks that must be completed as part of donating and incubating the
`solr-benchmark-workloads` repository under the Apache Software Foundation Solr PMC.

Items are roughly ordered by priority.

---

## Legal and Governance

- [ ] Complete and file the Software Grant Agreement (SGA) with the ASF for all contributed code
- [ ] Ensure all significant contributors have a signed Apache Individual Contributor License Agreement (ICLA) on file
- [ ] Move repository to the `apache/` GitHub organisation (`apache/solr-benchmark-workloads`)
- [ ] Update all internal GitHub links from `janhoy/` to `apache/` (README.md, CONTRIBUTING.md, USER_GUIDE.md)
- [ ] Add `DOAP` (Description of a Project) file if required by Solr PMC conventions
- [ ] Confirm data licenses (Creative Commons 3.0 for Geonames, public domain for NYC TLC) are compatible with ASF hosting

## Product / Project Naming

- [ ] Decide on and ratify the official project name with the Solr PMC (current working name: "Apache Solr Benchmark")
- [ ] Ensure the name does not conflict with existing ASF projects or trademarks
- [ ] Rename the `osbenchmark` Python package in `solr-benchmark` to a name that does not reference OpenSearch Benchmark
- [ ] After package rename, update the `osbenchmark` integration test paths referenced in `CONTRIBUTING.md`

## Data Hosting

- [ ] Identify and provision ASF-controlled or community-controlled storage for the corpus data files
  - Geonames corpus (~265 MB compressed): currently at `dbyiw3u3rf9yr.cloudfront.net/corpora/geonames`
  - NYC Taxis corpus (~4.8 GB compressed): currently at `dbyiw3u3rf9yr.cloudfront.net/corpora/nyc_taxis`
- [ ] Update `base-url` in `geonames/workload.json` and `nyc_taxis/workload.json` once data is re-hosted
- [ ] Remove the FIXME note in `README.md` once data hosting is resolved
- [ ] Document the data provenance and hosting arrangement in the project wiki or README

## Infrastructure and CI

- [ ] Set up GitHub Actions workflows for integration testing on the `apache/` repository
- [ ] Update the integration test configuration in `solr-benchmark` to point to the new `apache/solr-benchmark-workloads` repository
- [ ] Configure branch protection rules on `main` consistent with Solr PMC policy
- [ ] Ensure releases/tags follow ASF release policy (signed release artifacts, vote threads on dev list)

## Documentation

- [ ] Update the central documentation site URL once it moves to an `apache.org` domain
  - Currently: `https://janhoy.github.io/solr-benchmark/`
  - Target: `https://solr.apache.org/benchmark/` or similar
- [ ] Publish the project on the ASF project listing page
- [ ] Add a `CHANGELOG` or release notes file

## Community

- [ ] Subscribe to and use `dev@solr.apache.org` for all project discussions
- [ ] Announce the donation on `dev@solr.apache.org` and `general@incubator.apache.org` (if going through incubator)
- [ ] Identify initial PMC members and committers
- [ ] Establish a regular release cadence
