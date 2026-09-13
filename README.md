# Tools IWC Lab

[![Galaxy Tool Linting and Tests for push and PR](https://github.com/galaxyproject/tools-iwc-lab/actions/workflows/pr.yaml/badge.svg?branch=main)](https://github.com/galaxyproject/tools-iwc-lab/actions/workflows/pr.yaml?query=branch%3Amain)
[![Weekly global Tool Linting and Tests](https://github.com/galaxyproject/tools-iwc-lab/actions/workflows/ci.yaml/badge.svg?branch=main)](https://github.com/galaxyproject/tools-iwc-lab/actions/workflows/ci.yaml?query=branch%3Amain)

Sandbox counterpart to [galaxyproject/tools-iuc](https://github.com/galaxyproject/tools-iuc) for agentic Galaxy tool-wrapper experiments.

This repository follows the Tools IUC layout and CI conventions without publishing experiments under the production `iuc` Tool Shed owner. Pull requests are linted and tested with Planemo against Galaxy. Merges may be uploaded to the Test and Main Tool Sheds under the separate, explicitly experimental `iwc-lab` owner, but only after an administrator enables each destination.

## Repository layout

- `tools/` contains individual Galaxy tool repositories. Each publishable directory has a `.shed.yml` file.
- `tool_collections/`, `data_managers/`, `suites/`, and `macros/` are reserved for the corresponding Tools IUC structures as experiments require them.
- `.github/workflows/pr.yaml` discovers, lints, and tests changed tool repositories and performs opt-in Tool Shed deployment after a merge.
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

Experimental changes belong here, not in `tools-iuc`. Publication under `iwc-lab` does not make a wrapper an IUC release. Once an experiment is ready for IUC ownership, submit a clean contribution to the upstream repository and follow its normal review process.

## Deployment safety

Each Tool Shed destination is enabled independently:

- Test Tool Shed requires `ENABLE_TEST_TOOL_SHED_DEPLOY=true` and the `TTS_API_KEY` secret.
- Main Tool Shed requires `ENABLE_MAIN_TOOL_SHED_DEPLOY=true` and the `TS_API_KEY` secret.

Both keys must belong to the `iwc-lab` owner on their respective services. Test Tool Shed runs first and is best-effort, matching Tools IUC; Main Tool Shed failures fail the deployment job and are reported on the merged pull request. If neither destination is enabled, linting and tests still run and deployment is skipped.

See [docs/bootstrap-checklist.md](docs/bootstrap-checklist.md) for repository setup and [docs/upstream-sync.md](docs/upstream-sync.md) for the upstream snapshot and refresh procedure.

## Upstream snapshot

Infrastructure and the smoke-test wrapper were reviewed from `galaxyproject/tools-iuc` commit `d9991e8a01cbdd9006d1e91ce632e823d6f7a9e0` (2026-09-12). Files copied from Tools IUC remain subject to the repository's MIT license.
