# Repository bootstrap checklist

Core pull-request and scheduled CI need no user-managed secrets. GitHub supplies a short-lived `GITHUB_TOKEN`, and the workflows declare the permissions they need.

## Required before accepting contributions

- [ ] Create `galaxyproject/tools-iwc-lab` with `main` as its default branch.
- [ ] Enable GitHub Actions and allow the actions referenced by `.github/workflows/`.
- [ ] Set default workflow permissions to read-only; the workflows request narrow write permissions for individual jobs.
- [ ] Create the labels `skip-url-check`, `skip-version-check`, and `ready-for-review`.
- [ ] Protect `main`: require pull requests, at least one approving review, and the `Check workflow success` status check.
- [ ] Decide who owns lab review and add valid entries to `.github/CODEOWNERS`.
- [ ] Run the `Weekly global Tool Linting and Tests` workflow manually once and verify lint, test, artifact-combination, and summary jobs.

## Required for Test Tool Shed deployment

- [ ] Create or designate a non-production account named `iwc_lab` on <https://testtoolshed.g2.bx.psu.edu/>.
- [ ] Generate an API key for that Test Tool Shed account.
- [ ] Add the API key as the repository Actions secret `TTS_API_KEY`.
- [ ] Verify every lab `.shed.yml` uses `owner: iwc_lab` and a `remote_repository_url` below `galaxyproject/tools-iwc-lab`.
- [ ] Set the repository Actions variable `ENABLE_TEST_TOOL_SHED_DEPLOY` to `true` only after the account and metadata are verified.
- [ ] Merge or rerun a harmless fixture change and confirm it appears only under the `iwc_lab` owner on the Test Tool Shed.

Do not add a production `TS_API_KEY`. The lab workflow intentionally has no Main Tool Shed deployment step.

## Optional ChatOps

The `/run-all-tool-tests` comment command is disabled unless the `PAT` secret exists.

- [ ] Create a bot-owned classic PAT with `public_repo` and `read:org` access, or an equivalently scoped fine-grained token with access only to this repository and permission to create repository dispatch events.
- [ ] Ensure the token owner is a member of the `galaxyproject` organization and has write access to this repository.
- [ ] Store the token as the repository Actions secret `PAT`.
- [ ] Comment `/run-all-tool-tests` on a pull request and verify that it starts the weekly workflow and posts the result back to the pull request.

The upstream `ready-for-review` workflow also moves pull requests on an organization project board using `APP_CLIENT_ID` and `APP_PRIVATE_KEY`. That project-specific integration is deliberately not copied here. Add those secrets only if a lab project board and a narrowly scoped GitHub App are intentionally configured.

## Recommended repository settings

- [ ] Enable Dependabot alerts and GitHub Actions version updates.
- [ ] Enable secret scanning and push protection.
- [ ] Disable force pushes and branch deletion on `main`.
- [ ] Require conversation resolution if lab review volume warrants it.
- [ ] Keep the repository public so fork pull requests exercise the same untrusted-contributor path as Tools IUC.
- [ ] Add a short repository description and the `galaxy`, `galaxy-tools`, `planemo`, and `sandbox` topics.
