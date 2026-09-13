# Tools IWC Lab

[![Galaxy Tool Linting and Tests for push and PR](https://github.com/galaxyproject/tools-iwc-lab/actions/workflows/pr.yaml/badge.svg?branch=main)](https://github.com/galaxyproject/tools-iwc-lab/actions/workflows/pr.yaml?query=branch%3Amain)
[![Weekly global Tool Linting and Tests](https://github.com/galaxyproject/tools-iwc-lab/actions/workflows/ci.yaml/badge.svg?branch=main)](https://github.com/galaxyproject/tools-iwc-lab/actions/workflows/ci.yaml?query=branch%3Amain)

Sandbox counterpart to [galaxyproject/tools-iuc](https://github.com/galaxyproject/tools-iuc) for agentic Galaxy tool-wrapper experiments.

This repository follows the Tools IUC layout and CI conventions without publishing experiments under the production IUC Tool Shed owner. Pull requests are linted and tested with Planemo against Galaxy. Merges may be uploaded to the Test Tool Shed under the separate `iwc_lab` owner, but only after an administrator explicitly enables deployment. This repository never deploys to the Main Tool Shed.

## Repository layout

- `tools/` contains individual Galaxy tool repositories. Each publishable directory has a `.shed.yml` file.
- `tool_collections/`, `data_managers/`, `suites/`, and `macros/` are reserved for the corresponding Tools IUC structures as experiments require them.
- `.github/workflows/pr.yaml` discovers, lints, and tests changed tool repositories and performs the opt-in Test Tool Shed deployment after a merge.
- `.github/workflows/ci.yaml` runs the complete collection weekly and on demand.
- `.tt_skip` and `.tt_biocontainer_skip` record intentional exclusions from testing and BioContainer linting.

`tools/compose_text_param` is a small copy of the upstream IUC wrapper retained as an end-to-end CI fixture. Its Tool Shed metadata uses the lab owner and points back to this repository.

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md) and the [IUC standards and best practices](https://galaxy-iuc-standards.readthedocs.io/en/latest/). A typical local check is:

```bash
planemo shed_lint --tools --ensure_metadata --urls --biocontainers \
  --skip_file tools/compose_text_param/.lint_skip \
  --skip version_bumped tools/compose_text_param
planemo test tools/compose_text_param
```

Experimental changes belong here, not in `tools-iuc`. Once an experiment is ready for production, submit a clean contribution to the upstream repository and follow its normal review process; do not promote artifacts directly from this lab's Test Tool Shed owner.

## Deployment safety

Test Tool Shed deployment requires both:

1. the repository variable `ENABLE_TEST_TOOL_SHED_DEPLOY` set to `true`; and
2. the repository secret `TTS_API_KEY` belonging to the `iwc_lab` Test Tool Shed account.

Without both, linting and tests still run and deployment is skipped. There is intentionally no `TS_API_KEY` or Main Tool Shed deployment job.

See [docs/bootstrap-checklist.md](docs/bootstrap-checklist.md) for repository setup and [docs/upstream-sync.md](docs/upstream-sync.md) for the upstream snapshot and refresh procedure.

## Upstream snapshot

Infrastructure and the smoke-test wrapper were reviewed from `galaxyproject/tools-iuc` commit `d9991e8a01cbdd9006d1e91ce632e823d6f7a9e0` (2026-09-12). Files copied from Tools IUC remain subject to the repository's MIT license.
