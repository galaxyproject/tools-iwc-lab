# Contributing

This repository is a sandbox for developing and evaluating Galaxy tool wrappers with agentic workflows. Pull requests for new experiments, wrapper updates, tests, CI improvements, and documentation are welcome.

## Before opening a pull request

- Put each publishable tool repository below `tools/`, `tool_collections/`, or `data_managers/` and include an accurate `.shed.yml`.
- Use the Test Tool Shed owner `iwc_lab`; never use the production `iuc` owner in lab metadata.
- Follow the [IUC standards and best practices](https://galaxy-iuc-standards.readthedocs.io/).
- Include tests that exercise the behavior changed by the contribution.
- Run `planemo lint --biocontainers PATH` and `planemo test PATH` locally when practical.
- Keep generated or downloaded test data small. Do not commit secrets, API keys, credentials, or private datasets.
- Disclose whether AI generated or assisted the contribution in the pull request checklist.

The CI pipeline also checks Python formatting with flake8, R formatting with styler, repository metadata, and newly changed files larger than 1 MB.

## Promotion to Tools IUC

This repository is not a staging branch for automatic production publication. When an experiment is ready, prepare a focused pull request against [galaxyproject/tools-iuc](https://github.com/galaxyproject/tools-iuc) and follow its contribution and review requirements. Upstream reviewers should be able to assess the resulting wrapper without relying on lab-only history or infrastructure.

## Deployment

Only merges to `main` can deploy, and only to the Test Tool Shed. Deployment is additionally gated by `ENABLE_TEST_TOOL_SHED_DEPLOY=true` and the `TTS_API_KEY` repository secret. Contributors from forks do not receive deployment credentials.
