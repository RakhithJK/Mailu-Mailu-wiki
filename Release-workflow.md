# release workflow
WIP. First notes on what steps must be taken to do a new release.   
For now everything is derived from the 1.7 PR.
See [PR #1627](https://github.com/Mailu/Mailu/pull/1627) for all commits used for creating 1.7 release

All following information is derived from this PR.

## Tag & Branch
Branch the release off from master. Tag this and push the tag

## CHANGELOG.md
change logs (for changelog.md) are generated using the python package [towncrier](https://pypi.org/project/towncrier/).

I think towncrier is run manually and the output is manually added to CHANGELOG.md (in root of project)
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
After infra is updated, the release will actually be available via mailu.io.

## Change stats endpoint
This was forgotten from ~1.4 - 1.7. Change
https://github.com/Mailu/Mailu/blob/master/core/admin/mailu/configuration.py#L27
to e.g. `18.{}.stats.mailu.io`

## Summarized
- [ ] Generate towncrier changelog and add it to CHANGELOG.md
- [ ] Write short upgrade instructions for CHANGELOG.md
- [ ] Add verbose written changelog for users to docs/releases.rst
- [ ] Update docs/setup.rst with new stable version. (replace the text 'x.x features the most...')
- [ ] Update docs/conf.py
  - [ ] `versions:` with new version if it is not already listed here.
  - [ ] `stable_version:` to new stable version.
- [ ] Additional steps for Infrastructure project.
  - [ ]   Update docs/.env with new version
  - [ ]   Update setup/.env with new version

# TODO
- [ ] Figure out how a new version is build and published to docker hub. Confirm that BORS(?) or another bot automatically picks up new branches and simply starts building when there is an update for a branch. When a build for a branch succeeds, it is automatically pushed to docker hub. This suspicion is based on the fact that each branch is an image tag in the docker repository. Whenever a PR is merged we also see a new image on the docker repository.
- [ ] Infrastructure project is used for mailu.io and setup.mailu.io. Check If I missed anything that is required for updating infra project.