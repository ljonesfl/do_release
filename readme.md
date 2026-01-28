# release-script
Shell script to automate the git flow release process.
Runs tests (python, ruby, php) before starting the release.


# Prerequisites 
Install [bump](https://github.com/ljonesfl/bump)

# Installation (MacOS)

	chmod +x do_release
	cp do_release /usr/local/bin
	chmod +x do_hotfix
	cp do_hotfix /usr/local/bin

# Usage

`do_release`

This will:
* Run tests.
* Perform the entire git flow release process.
* Prepare the next version (if not using a date strategy) assuming it is a minor release.

The main branch name is detected automatically (either `main` or `master`). 
If you want to override it, use the `--branch` option.


## Tests

If there are no tests, the script will skip running them.
If there are tests, they will be run before starting the release.
If any test fails, the release will be aborted.

If you want to skip running the tests run:

`do_release --skip-tests`

To test environment detection, run:

`do_release --show-environment`

## Branch Name Override

If for some reason, branch detection isn't working, run:
`do_release --branch=main`

# The Automated Process

* Runs tests (unless skipped or not present)
* git checkout {main|master}
* git pull
* get checkout develop
* git flow release start {version} (pulled from .version.json)
* Update versionlog.md (if date strategy, add the date version, if semver, append the release date to the header)
* git flow release finish
* git checkout {main|master}
* git push
* git push --tags
* git checkout develop
* if not date strategy, bump version to next minor, update versionlog.md
* git push

# Hotfix Usage

Start a hotfix:

`do_hotfix --start`

Finish a hotfix:

`do_hotfix --finish`

The main branch name is detected automatically (either `main` or `master`).
If you want to override it, use the `--branch` option.

## Hotfix Tests

Tests are run during `--finish` (not during `--start`).
If there are no tests, the script will skip running them.
If any test fails, the hotfix will be aborted.

To skip running the tests during finish:

`do_hotfix --finish --skip-tests`

To test environment detection, run:

`do_hotfix --show-environment`

## Hotfix Branch Name Override

If for some reason, branch detection isn't working, run:
`do_hotfix --branch=main --start`

# The Hotfix Process

Start stage:
* git checkout {main|master}
* git pull
* git flow hotfix start {version} (patch bump via .version.json)

Finish stage:
* Run tests (unless skipped or not present)
* Update versionlog.md (if date strategy, add the date version, if semver, append the release date to the header)
* git flow hotfix finish
* git checkout {main|master}
* git push
* git push --tags
* git checkout develop
* git push
