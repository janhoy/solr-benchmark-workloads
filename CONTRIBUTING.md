# Contributor Guidelines

This repository contains the default workload specifications for
[Apache Solr Benchmark](https://github.com/janhoy/solr-benchmark).
This document is a general guide on best practices for contributing to this repository.

## Contents

- [Before you start](#before-you-start)
- [Contributing a change to existing workload(s)](#contributing-a-change-to-existing-workloads)
- [Test changes](#test-changes)
  - [Testing changes locally](#testing-changes-locally)
  - [Testing changes with integration tests](#testing-changes-with-integration-tests)
- [Publish changes in a pull-request](#publish-changes-in-a-pull-request)
- [Reviewing pull-requests](#reviewing-pull-requests)
  - [Backporting](#backporting)
- [Contributing a workload](#contributing-a-workload)


## Before you start

This is an Apache Software Foundation project. All contributors must have a signed
[Apache Individual Contributor License Agreement (ICLA)](https://www.apache.org/licenses/icla.pdf)
on file with the ASF before any contributions can be merged.

For significant new features or design changes, it is recommended to first raise a discussion
on the [dev@solr.apache.org](https://lists.apache.org/list.html?dev@solr.apache.org) mailing list
or open a GitHub issue so the community can provide early feedback.

All commits should be signed off with the `--signoff` flag (i.e. `git commit --signoff`) to
certify that you wrote the patch or otherwise have the right to submit it under the project's
open-source license.


## Contributing a change to existing workload(s)

Before making a change, fork this repository and make the change on a feature branch.

- If your change is only relevant to a specific Solr version branch, base the feature branch off
  that branch (e.g. `solr-9`).
- If the change applies to all versions, base the feature branch off `main` and backport as
  needed once merged.


## Test changes

After making changes in your feature branch, test them locally and optionally via GitHub Actions
integration tests in your forked `solr-benchmark` repository.

### Testing changes locally

1. Start a local Solr cluster to test against (standalone or SolrCloud).
2. Run `solr-benchmark` pointing at your modified workload using `--workload-path` or
   `--workloads-repository`. Use `--test-mode` for a quick sanity-check run:

```bash
solr-benchmark run \
  --pipeline=benchmark-only \
  --target-host=localhost:8983 \
  --workload-path=/path/to/your/fork/nyc_taxis \
  --test-mode
```

3. Verify the benchmark completes successfully and produces the expected output.

Additional tips:
- `--test-mode` reduces the corpus size and iteration counts so the run finishes quickly.
- To enforce a specific workloads branch from a remote repository, pass
  `--workloads-repository=https://github.com/<YOUR USERNAME>/solr-benchmark-workloads` and
  `--distribution-version=X.Y.Z` to pin `solr-benchmark` to the matching branch.

### Testing changes with integration tests

To catch regressions across the full suite, run integration tests from your forked
`solr-benchmark` repository.

**One-time setup:**

1. Fork [solr-benchmark](https://github.com/janhoy/solr-benchmark).
2. In your fork, create a branch called `test-forked-workloads` based off `main`.
3. In that branch, update `benchmark-os-it.ini` and `benchmark-in-memory.ini` under
   `/osbenchmark/it/resources/` to point at your forked workloads repository:

```ini
[workloads]
default.url = https://github.com/<YOUR GITHUB USERNAME>/solr-benchmark-workloads
```

4. Push that branch to your fork.

**Running the tests:**

1. Cherry-pick your workload change(s) onto the relevant branches of your forked workloads
   repository.
2. Push those branches.
3. In your forked `solr-benchmark` repository, go to **GitHub Actions → Run Integration Tests**,
   select the `test-forked-workloads` branch, and click **Run workflow**.
4. Verify that all tests pass.


## Publish changes in a pull-request

Before opening a pull request, make sure you have addressed the following:

1. **Describe the changes**: Explain what the change does and what problem it solves. For bug
   fixes, include a before/after comparison. For new features, show what users can expect.
2. **Indicate where to backport**: Note whether the change should be merged into `main` only or
   also backported to one or more version branches (e.g. `solr-9`, `solr-10`).
3. **Provide evidence of testing**: Paste a short sample output from your local test run, or link
   to a successful GitHub Actions run in your fork.
4. **Request review**: For changes that touch workload correctness or Solr-version-specific
   behaviour, tag a subject-matter expert.

Create a pull request from your fork to the
[`main` branch of this repository](https://github.com/janhoy/solr-benchmark-workloads).


## Reviewing pull-requests

Reviewers and maintainers should:

1. Review the diff for correctness and scope — changes should be well-defined and minimal.
2. Confirm the change has been tested (local run output or CI link).
3. Confirm the PR description specifies which branches to backport.
4. For workload correctness questions, ensure a subject-matter expert has reviewed.
5. Label the PR with the appropriate backport labels before approving.


### Backporting

If the workload repository has Solr-version branches, changes should be cherry-picked from
`main` to the most recent supported branch and backward from there. For example:

```
main → solr-10 → solr-9
```

In the event of a merge conflict during backporting, open a separate pull request that applies
the change directly to the target branch. Ensure **only** the changes from the original PR are
included in the backport PR.


## Contributing a workload

See [USER_GUIDE.md](USER_GUIDE.md) for the structure of a workload, the role of the shared
`common_operations/` library, and tips on writing test procedures.

For questions about contributing a workload, reach out on the
[dev@solr.apache.org](https://lists.apache.org/list.html?dev@solr.apache.org) mailing list or
open a [GitHub issue](https://github.com/janhoy/solr-benchmark-workloads/issues).
