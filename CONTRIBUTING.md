# Contributing to SQLLineage

Before start, thanks for taking the time to contribute. Always happy to see more people interested in
this project.

The following is a set of guidelines for contributing to SQLLineage, and we try our best to enforce these guidelines
with CI tools.

## Table of Contents

* [Style Guide](#style-guide)
  * [black](#black)
  * [flake8](#flake8)
  * [mypy](#mypy)
  * [pytest](#pytest)
* [Set up Local Development Environment](#set-up-local-development-environment)
  * [Running CI in Local](#running-ci-in-local)
  * [Multi Python Version](#multi-python-version)
  * [Pre-commit](#pre-commit)
* [Development Flow](#development-flow)
  * [Raise Issue](#raise-issue)
  * [Issue Handling](#issue-handling)
  * [Submit Pull Request](#submit-pull-request)
* [Release](#release)
  * [Milestone](#milestone)
  * [Tag](#tag)

## Style Guide

### black

Make sure you follow [black](https://github.com/psf/black) code format.

### flake8

We're enforcing [flake8](https://gitlab.com/pycqa/flake8), together with plugins including `flake8-blind-except`,
`flake8-builtins`, `flake8-import-order`, `flake8-logging-format` to keep bad code smell away.

### mypy

Since we're writing a library which requires Python 3, type annotation is heavily used to ensure a more static typed
style of code. So [mypy](https://github.com/python/mypy) as a static type checker is our choice to detect possible
type issues.

### pytest

SQLLineage is based on `sqlparser`, which itself is a non-validating SQL parser, supporting a variety of SQL dialects.
This at the same time means `sqlparser` won't be 100% correct in recognizing each dialect. So unit test is particularly
important in the success of SQLLineage. We have a comprehensive test suite, orchestrated by
[pytest](https://github.com/pytest-dev/pytest) with [coveragepy](https://github.com/nedbat/coveragepy) to validate
each test case.

## Set up Local Development Environment

All the style guidelines are enforced by [GitHub Actions](https://github.com/reata/sqllineage/actions), so each time you
submit a PR or push a new commit, these server side checks will be triggered, but you can also set up local CI for
rapid validation of your code change.

### Create virtual environment

Some build configuration assumes that development takes place in a virtual environment, named `venv`.
Set yourself up for this using the following commands:

```bash
python -m venv venv
. venv/bin/activate
```

### Running CI in Local

The entrypoint for CI in GitHub Actions is simply [tox](https://github.com/tox-dev/tox). Install `tox` and run it in local,
all the style check will be triggered.

Change the Python version accordingly to your local environment, for example:

```bash
tox -e py313
```

### Multi Python Version

We rely on [GitHub Actions matrix](https://github.com/reata/sqllineage/blob/master/.github/workflows/python-package.yml)
to do multiple Python version test and feed the exact version to `tox`, same as you saw in the local mode here.
However, in case you would also like to test against multiple Python versions locally, you can install different
versions of Python, possibly with [pyenv](https://github.com/pyenv/pyenv) in local environment,
and run `tox` without specifying a Python version.

In this case, all Python versions defined in [tox.ini](https://github.com/reata/sqllineage/blob/master/tox.ini) will
be tested.

```bash
tox
```

### Pre-commit

[Pre-commit](https://github.com/pre-commit/pre-commit) is yet another way to do some checks in client side. We offer
this [.pre-commit-config.yaml](https://github.com/reata/sqllineage/blob/master/.pre-commit-config.yaml) for `black`, `flake8`
and `mypy` checks. You can set it up with

```bash
pip install pre-commit
pre-commit install
```

Once setup, each time you make a new commit in the local (even before pushing to remote), this pre-commit hook will be
triggered to help you.

## Development Flow

### Raise Issue

We use [GitHub Issues](https://github.com/reata/sqllineage/issues) to manage the development of SQLLineage projects.
Any development work, be it a new feature, enhancement for existing functionality or bug tracing, should start with an
issue.

Project owner will regularly review the issue list, close unreasonable ones and attach a label/milestone to the rest.

### Issue Handling

Always add comment to the issue you're working on. For a new feature, write an outline of your implementation. For bugs,
write down the root cause analysis, and your proposed fix.

We use [GitHub Flow](https://guides.github.com/introduction/flow/) as Git workflow. Just fork this repo and create your
own branch, continue working on it before submit a PR request.

### Submit Pull Request

Once you finish the development in your forked repo, you're ready for a pull request. Before submitting, please make sure:

* The last commit of your branch passed GitHub Actions.
* Your branch has only ahead with upstream master branch, no commits fall behinds. If otherwise, please rebase upstream
  master, and fix the conflicts if any.
  
## Release

### Milestone

We use [Semantic Versioning](https://semver.org/) to manage version. Major/Minor version should be traced with a
[milestone](https://github.com/reata/sqllineage/milestones), which contains a bunch of issues. Make sure all issues are
closed before closing the milestone.

### Tag

Whenever you need to do a release, remember to update CHANGELOG, the version for both `sqllineage` and `sqllineagejs`, and
then tag that commit by creating a [release](https://github.com/reata/sqllineage/releases/new).
A [Python package GitHub Action](https://github.com/reata/sqllineage/blob/master/.github/workflows/python-package.yml)
is set up to automatically upload the whl and tar.gz to PyPI once a new release is created.
