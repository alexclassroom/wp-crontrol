# Contributing to WP Crontrol

Bug reports, code contributions, and general feedback are very welcome. These should be submitted through [the GitHub repository](https://github.com/johnbillion/wp-crontrol). Development happens in the `develop` branch, and any pull requests should be made against that branch please.

* [Reviews on WordPress.org](#reviews-on-wordpressorg)
* [Reporting Security Issues](#reporting-security-issues)
* [Inclusivity and Code of Conduct](#inclusivity-and-code-of-conduct)
* [Setting up Locally](#setting-up-locally)
* [Running the Tests](#running-the-tests)
* [Releasing a New Version](#releasing-a-new-version)

## Reviews on WordPress.org

If you enjoy using WP Crontrol I would greatly appreciate it <a href="https://wordpress.org/support/plugin/wp-crontrol/reviews/">if you left a positive review on the WordPress.org Plugin Directory</a>. This is the fastest and easiest way to contribute to WP Crontrol 😄.

## Reporting Security Issues

You can report security bugs through the Patchstack Vulnerability Disclosure Program. The Patchstack team helps validate, triage, and handle any security vulnerabilities. [Report a security vulnerability here](https://patchstack.com/database/vdp/wp-crontrol).

Do not report security issues on GitHub or the WordPress.org support forums. Thank you.

## Inclusivity and Code of Conduct

Contributions to WP Crontrol are welcome from anyone. Whether you are new to Open Source or a seasoned veteran, all constructive contribution is welcome and I'll endeavour to support you when I can.

This project is released with <a href="https://github.com/johnbillion/wp-crontrol/blob/develop/CODE_OF_CONDUCT.md">a contributor code of conduct</a> and by participating in this project you agree to abide by its terms. The code of conduct is nothing to worry about, if you are a respectful human being then all will be good.

## Setting up Locally

You can clone this repo into your `wp-content/plugins` directory but you'll need to install the development dependencies before activating the plugin.

### Prerequisites

* [Composer](https://getcomposer.org/)
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) if you want to run the tests
* [Node](https://nodejs.org/) if you are packaging a release

### Setup

Install the PHP dependencies:

	composer install

## Running the Tests

The test suite includes acceptance tests which run in a Docker container. Ensure Docker Desktop is running before running the tests.

To run the whole test suite which includes PHPCS code sniffs, PHPStan static analysis, and WPBrowser acceptance tests:

	composer test

To run just the code sniffs:

	composer test:cs

To run just the static analysis:

	composer test:phpstan

To run just the acceptance tests:

	composer test:acceptance

If you're repeatedly running the tests during development, to speed things up you can start and stop the containers manually and then they won't be started and stopped each time you run the tests:

	composer test:start
	composer test
	composer test
	composer test
	composer test:stop

## Releasing a New Version

These are the steps to take to release a new version of WP Crontrol (for contributors who have push access to the GitHub repo).

### Prior to Release

1. Check [the milestone on GitHub](https://github.com/johnbillion/wp-crontrol/milestones) for open issues or PRs. Fix or reassign as necessary.
1. If this is a non-patch release, check issues and PRs assigned to the patch or minor milestones that will get skipped. Reassign as necessary.
1. Ensure you're on the `develop` branch and all the changes for this release have been merged in.
1. Ensure `readme.md` contains up to date descriptions, "Tested up to" versions, FAQs, screenshots, etc.
1. Ensure `.gitattributes` is up to date with all files that shouldn't be part of the build.
   - To do this, export the archive then check the contents for files that shouldn't be part of the package:

         git archive --output=wp-crontrol.zip HEAD

1. Run the tests and ensure everything passes:

       composer test

1. Prepare a changelog for [the Releases page on GitHub](https://github.com/johnbillion/wp-crontrol/releases).

### For Release

1. Install the Node dependencies:

       npm install

1. Bump the plugin version number:
   - For a patch release (1.2.3 => 1.2.4):

         npm run bump:patch

   - For a minor release (1.2.3 => 1.3.0):

         npm run bump:minor

   - For a major release (1.2.3 => 2.0.0):

         npm run bump:major

1.     git push origin develop
1. Wait until (and ensure that) [the build passes](https://github.com/johnbillion/wp-crontrol/actions)
1.     git checkout master
1.     git merge develop
1.     git push origin master
1.     git push origin master:release
1. Wait for [the Build Release action](https://github.com/johnbillion/wp-crontrol/actions?query=workflow%3A%22Build+Release%22) to complete
1. Enter the changelog into [the release on GitHub](https://github.com/johnbillion/wp-crontrol/releases) and publish it.

### Post Release

Publishing a release on GitHub triggers an action which deploys the release to the WordPress.org Plugin Directory. No need to touch Subversion.

New milestones are automatically created for the next major, minor, and patch releases where appropriate.

1. Close the milestone.
1. If this is a non-patch release, manually delete any [unused patch and minor milestones on GitHub](https://github.com/johnbillion/wp-crontrol/milestones).
1. Check the new version has appeared [on the WordPress.org plugin page](https://wordpress.org/plugins/wp-crontrol/) (it'll take a few minutes).
1. Resolve relevant threads on [the plugin's support forums](https://wordpress.org/support/plugin/wp-crontrol/).
1. Consume tea and cake as necessary.

### Asset Updates

Assets such as screenshots and banners are stored in the `.wordpress-org` directory. These get deployed as part of the automated release process too.

In order to deploy only changes to assets, push the change to the `deploy` branch and they will be deployed if they're the only changes in the branch since the last release. This allows for the "Tested up to" value to be bumped as well as assets to be updated in between releases.
