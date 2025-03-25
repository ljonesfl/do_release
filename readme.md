# release-script
Shell script to automate the git flow release process.

# Prerequisites 
Install [bump](https://github.com/ljonesfl/bump)

# Usage

`do_release`

This will perform the entire git flow release process as well as prepare the next version,
assuming it is a minor release.

If you want to skip running the tests run:

`do_release --skip-tests`

To test the environment detection, run:

`do_release --show-environment`

