Work in progress. Purpose of this page is to offer a checklist for reviewing PRs. Once this is more finalised we will also update the main documentation with below information.

This article describes all metrics that must be used for reviewing a pull request. It contains a checklist to help you not overlook some things.

## Checklist

* PR is for branch master. Only pull requests on master are accepted.
  * For core contributors under special circumstances an exception can be made. But it needs good motivation. E.g. a defect that only happens on this branch.
* A newsfragment is present (https://mailu.io/master/contributors/workflow.html#changelog). 
  * Normally every PR requires a newsfragment. This will be part of the eventual changelog.md and release notes. Without this newsfragment nobody knows that this PR was merged with the project.
  * Only for very minor changes (E.g. typo fixed in documentation), we do not require a newsfragment.
  * Creating a newsfragment is the standard. Not providing it is the exception.
* Documentation is updated
  * Features / enhancements that change the functioning of the software, result normally in documentation that must be updated.
  * Have a quick look at the relevant documentation and ask yourself if this is still sufficient after merging this PR.
* Actually read the documentation via a running documentation container.
  * Sometimes it is rendered differently than expected. Actually reading the modified docs via the running documentation container helps catching this.
* PR is for a single feature or bug.
* PEP8 compliant. 
  * In the future we will have github actions checking this. Does the workflow state it is PEP8 compliant?
  * PR can only be merged if it is compliant. Any findings must be addressed.
* Backporting
  * PR can be backported to the stable branch by adding the label type/backport
  * Urgent fixes can be backported to the stable branch.
  * In general only for bug fixes, not for new features / enhancements.
    * If you want to deviate for this, please discuss this first in the Mailu Dev chat (matrix) or @mention other core contributors in the PR.

## Testing PR
CI workflow pushes all testing images to `mailuci/<name>:pr-<no>`
E.g. to test PR #500
```
export DOCKER_ORG="mailuci"
export MAILU_VERSION="pr-500"
docker-compose pull
docker-compose up -d
```

## Testing/Reading documentation changes
Two examples for running the documentation image.

```
#For checked out PR repo
cd <Mailu repo>
docker-compose -f tests/build.yml build docs
echo visit http://127.0.0.1:1234/local/
docker run --rm -p 127.0.0.1:1234:80 docs
```
```
#For testing PR documentation image
#Replace pr-#### with the PR number. 
#E.g. for PR 2021 the tag is pr-2021.
echo visit http://127.0.0.1:1234/pr-####/
docker run --rm -p 127.0.0.1:1234:80 mailuci/docs:pr-####
```

## Quickly checking out code PR
With github  CLI it is very easy to check out the PR branch.

1. Install github cli (https://github.com/cli/cli/blob/trunk/docs/install_linux.md)
2. Add your SSH key to github and configure this on your machine (https://docs.github.com/en/authentication/connecting-to-github-with-ssh).
3. You could use the below script to checkout PRs:
```
#!/bin/bash

echo "Script will delete the folder Mailu, clone the Mailu project and then get the PR patch"
echo "Press enter to continue or CTRL+C to cancel"
read press

sudo rm -r Mailu
gh repo clone Mailu/Mailu

echo "Please enter the PR number and continue with Enter."
read -p 'PR number: ' pr

echo "gh pr checkout $pr"
pushd Mailu
gh pr checkout $pr
popd

echo "Done! Good luck with reviewing PR $pr"
exit 0
```