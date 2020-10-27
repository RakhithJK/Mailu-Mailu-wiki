# release workflow
WIP. First notes on what steps must be taken to do a new release.   
For now everything is derived from the 1.7 PR.
See [PR #1627](https://github.com/Mailu/Mailu/pull/1627) for all commits used for creating 1.7 release

All following information is derived from this PR.

## New Branch and update Travis
Create a Branch for the new release (e.g. 1.9).
Update Travis to pick up the new branch in .travis.yml in root folder of project. See section Branches: in travis.yml.
Update Mergify.yml with new version.
Change the description of the type/backport label.

## CHANGELOG.md
change logs (for changelog.md) are generated using the python package [towncrier](https://pypi.org/project/towncrier/).

I think towncrier is run manually and the output is manually added to CHANGELOG.md (in root of project)
The project root already contains a config file pyproject.toml for towncrier. Possibly this config file must be provided to the towncrier module.  
CHANGELOG.md should also contain migration notes (added manually)

## release notes (docs/releases.rst)
The release notes for the doc site are written manually. So someone must think of a description to put in releases.rst to describe what is new in the new release. What challenges we had and any other info we want to share with the outside  world in regard to the release.

## Add new version to docs
New version is added to the docs by modifying in docs/conf.py the setting
`'stable_version': '1.7',` to the new version.
Note that the `versions` dictionary in this file contains all versions for dropdown box.

## Add new version to docs/setup.rst
line 44: 
change
` ``1.7`` features the most recent stable version for Mailu.`
to
` ``1.8`` features the most recent stable version for Mailu.`

## In infra project update version numbers
See https://github.com/Mailu/infra/commit/626a7f4304a1d964a9d8f1cd12d2a920bad42291  
Update docs/.env with new version  
Update setup/.env with new version 
Note: When still testing the new version, the env variable stable_version must be kept on the previous release.
Only when the new version is considered stable, change stable_version to the new version and comment out the ${TEST} image in the docker-compose.yml file.
After infra is updated, the release will actually be available via mailu.io.

## Change stats endpoint
This was forgotten from ~1.4 - 1.7. Change
https://github.com/Mailu/Mailu/blob/master/core/admin/mailu/configuration.py#L27
to e.g. `18.{}.stats.mailu.io`

## Summarized
- [ ] Create new branch for release.
- [ ] Update Travis.yml to use the new branch.
- [ ] Update mergify with new version/branch.
- [ ] Generate towncrier changelog and add it to CHANGELOG.md
- [ ] Write short upgrade instructions for CHANGELOG.md
- [ ] Add verbose written changelog for users to docs/releases.rst
- [ ] Update docs/setup.rst with new stable version. (replace the text 'x.x features the most...')
- [ ] Update docs/conf.py
  - [ ] `versions:` with new version if it is not already listed here.
  - [ ] `stable_version:` to new stable version.
- [ ] Update stats endpoint url for new release.
- [ ] Additional steps for Infrastructure project.
  - [ ]   Update docs/.env with new version
  - [ ]   Update setup/.env with new version
