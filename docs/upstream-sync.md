# Upstream synchronization

The initial scaffold was derived from `galaxyproject/tools-iuc` commit `d9991e8a01cbdd9006d1e91ce632e823d6f7a9e0`, committed on 2026-09-12.

## Mirrored infrastructure

- pull-request discovery, Planemo lint/test, flake8, R styler, file-size checks, artifact combination, and required-check aggregation;
- weekly full-repository Planemo lint/test;
- fallback CI for documentation and infrastructure-only pull requests;
- Dependabot configuration, pull request template, flake8 configuration, ignore files, license, and code of conduct;
- optional slash-command dispatch and ready-for-review labeling;
- one small tool wrapper as an end-to-end fixture.

## Intentional differences

- Production Tool Shed publication is removed.
- Test Tool Shed publication is disabled by default and uses the isolated `iwc_lab` owner.
- IUC-specific code owners and organization project-board automation are not copied.
- The lab starts with a single fixture instead of duplicating the entire production tool catalog.
- Lab documentation makes experimental status and promotion boundaries explicit.

## Refresh procedure

1. Fetch the latest `main` branch from <https://github.com/galaxyproject/tools-iuc> into a temporary clone.
2. Compare `.github/`, root configuration files, `CONTRIBUTING.md`, and any shared helper scripts with this repository.
3. Port generally useful CI changes while preserving the deployment gate, the `iwc_lab` owner, and the absence of Main Tool Shed publication.
4. Update the snapshot commit in this file and `README.md`.
5. Validate workflow syntax and run the pull-request and weekly workflows before enabling deployment.

Treat upstream tool content separately from infrastructure. Copy only the wrappers needed for a specific experiment and retain their license and provenance.
