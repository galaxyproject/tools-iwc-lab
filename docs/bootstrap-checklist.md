# Repository bootstrap checklist

Core pull-request and scheduled CI need no user-managed secrets. GitHub supplies a short-lived `GITHUB_TOKEN`, and the workflows declare the permissions they need.

Repository settings below were last verified on 2026-09-13. Checked items are already configured; unchecked items require an owner decision or credential that was deliberately not created during bootstrap.

## Required before accepting contributions

- [x] Create `galaxyproject/tools-iwc-lab` with `main` as its default branch.
- [x] Enable GitHub Actions and allow the actions referenced by `.github/workflows/`.
- [x] Set default workflow permissions to read-only; the workflows request narrow write permissions for individual jobs.
- [x] Create the labels `skip-url-check`, `skip-version-check`, and `ready-for-review`.
- [x] Protect `main`: require pull requests, at least one approving review, the `Check workflow success` status check, and resolved review conversations; disable force pushes and branch deletion.
- [ ] Decide who owns lab review and add valid entries to `.github/CODEOWNERS`.
- [x] Run the `Weekly global Tool Linting and Tests` workflow manually once and verify lint, test, artifact-combination, and summary jobs ([successful bootstrap run](https://github.com/galaxyproject/tools-iwc-lab/actions/runs/34733940362)).

## Required for Test Tool Shed deployment

- [x] Create or designate a non-production account named `iwc-lab` on <https://testtoolshed.g2.bx.psu.edu/>.
- [ ] Generate an API key for that Test Tool Shed account.
- [ ] Add the API key as the repository Actions secret `TTS_API_KEY`.
- [x] Verify every lab `.shed.yml` uses `owner: iwc-lab` and a `remote_repository_url` below `galaxyproject/tools-iwc-lab`.
- [ ] Set the repository Actions variable `ENABLE_TEST_TOOL_SHED_DEPLOY` to `true` only after the account and metadata are verified.
- [ ] Merge or rerun a harmless fixture change and confirm it appears under the `iwc-lab` owner on the Test Tool Shed.

## Required for Main Tool Shed deployment

- [x] Create or designate the explicitly experimental `iwc-lab` owner on <https://toolshed.g2.bx.psu.edu/>.
- [ ] Generate an API key for that Main Tool Shed account.
- [ ] Add the API key as the repository Actions secret `TS_API_KEY`.
- [ ] Set the repository Actions variable `ENABLE_MAIN_TOOL_SHED_DEPLOY` to `true` only after the account and metadata are verified.
- [ ] Merge or rerun a harmless fixture change and confirm it appears under the `iwc-lab` owner on the Main Tool Shed.

The two services issue independent API keys. Never put an `iuc` owner credential in this repository.

## Optional ChatOps

The `/run-all-tool-tests` comment command is disabled unless the `PAT` secret exists.

- [ ] Create a bot-owned classic PAT with `public_repo` and `read:org` access, or an equivalently scoped fine-grained token with access only to this repository and permission to create repository dispatch events.
- [ ] Ensure the token owner is a member of the `galaxyproject` organization and has write access to this repository.
- [ ] Store the token as the repository Actions secret `PAT`.
- [ ] Comment `/run-all-tool-tests` on a pull request and verify that it starts the weekly workflow and posts the result back to the pull request.

The upstream `ready-for-review` workflow also moves pull requests on an organization project board using `APP_CLIENT_ID` and `APP_PRIVATE_KEY`. That project-specific integration is deliberately not copied here. Add those secrets only if a lab project board and a narrowly scoped GitHub App are intentionally configured.

## Recommended repository settings

- [x] Enable Dependabot alerts, security updates, and GitHub Actions version updates.
- [x] Enable secret scanning and push protection.
- [x] Disable force pushes and branch deletion on `main`.
- [x] Require conversation resolution.
- [x] Keep the repository public so fork pull requests exercise the same untrusted-contributor path as Tools IUC.
- [x] Add a short repository description and the `galaxy`, `galaxy-tools`, `planemo`, and `sandbox` topics.

No Actions secrets, variables, or environments were configured during bootstrap. Tool Shed publication and optional ChatOps therefore remain safely disabled until their corresponding credentials and gates are added.
