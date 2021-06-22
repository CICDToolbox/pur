<h1 align="center">
    <a href="https://github.com/WolfSoftware">
        <img src="https://raw.githubusercontent.com/WolfSoftware/branding/master/images/general/banners/64/black-and-white.png" alt="Wolf Software Logo" />
    </a>
    <br />
    Pur for CI/CD Pipelines
</h1>

<p align="center">
    <a href="https://github.com/CICDToolbox/pur/actions/workflows/pipeline.yml">
        <img src="https://img.shields.io/github/workflow/status/CICDToolbox/pur/pipeline/master?logo=github&logoColor=white&style=for-the-badge" alt="Github Build Status">
    </a>
    <a href="https://travis-ci.com/CICDToolbox/pur">
        <img src="https://img.shields.io/travis/com/CICDToolbox/pur/master?style=for-the-badge&logo=travis" alt="Travis Build Status">
    </a>
    <a href="https://github.com/CICDToolbox/pur/releases/latest">
        <img src="https://img.shields.io/github/v/release/CICDToolbox/pur?color=blue&style=for-the-badge&logo=github&logoColor=white&label=Latest%20Release" alt="Release">
    </a>
    <a href="https://github.com/CICDToolbox/pur/releases/latest">
        <img src="https://img.shields.io/github/commits-since/CICDToolbox/pur/latest.svg?color=blue&style=for-the-badge&logo=github&logoColor=white" alt="Commits since release">
    </a>
    <br />
    <a href=".github/CODE_OF_CONDUCT.md">
        <img src="https://img.shields.io/badge/Code%20of%20Conduct-blue?style=for-the-badge&logo=read-the-docs&logoColor=white" />
    </a>
    <a href=".github/CONTRIBUTING.md">
        <img src="https://img.shields.io/badge/Contributing-blue?style=for-the-badge&logo=read-the-docs&logoColor=white" />
    </a>
    <a href=".github/SECURITY.md">
        <img src="https://img.shields.io/badge/Report%20Security%20Concern-blue?style=for-the-badge&logo=read-the-docs&logoColor=white" />
    </a>
    <a href="https://github.com/CICDToolbox/pur/issues">
        <img src="https://img.shields.io/badge/Get%20Support-blue?style=for-the-badge&logo=read-the-docs&logoColor=white" />
    </a>
    <br />
    <a href="https://github.com/TGWolf">
        <img src="https://img.shields.io/badge/Created%20by%20Wolf-black?style=for-the-badge" />
    </a>
</p>

## Table of Contents
* [Overview](#overview)
* [Usage](#usage)
    * [Basic Usage](#basic-usage)
    * [Showing Errors](#showing-errors)
    * [Skipping Modules](#skipping-modules)
    * [Dry Run](#dry-run)
    * [Multiple Options](#multiple-options)
* [Example Output](#example-output)
* [File Identification](#file-identification)
* [Show Support](#show-support)

<a name="overview"></a>
## Overview

A tool to check your Python projects requirements.txt for updates in CI/CD pipelines using [pur](https://pypi.org/project/pur/).

This tool has been written and tested using both GitHub Actions and Travis CI, but it should work out of the box with a lot of other CI/CD tools.

<a name="usage"></a>
## Usage

<a name="basic-usage"></a>
### Basic Usage

The most basic usage is to simply execute the pipeline, this will scan all requirement.txt files to ensure they are running the latest versions of all required modules.
It will error if older versions are configured.

#### GitHub Actions

```yml
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    - name: Set up Python 3.9
      uses: actions/setup-python@v2
      with:
        ruby-version: 3.9
    - name: Run Pur
      run: wget --quiet -O - https://raw.githubusercontent.com/CICDToolbox/pur/master/pipeline.sh | bash
```

#### Travis CI

```yml
language: python
python: 3.9

script:
  - wget --quiet -O - https://raw.githubusercontent.com/CICDToolbox/pur/master/pipeline.sh | bash
```

<a name="showing-errors"></a>
### Showing Errors

By default the specific content of the errors found are not shown only the fact an error was found. You can enable the showing of errors by setting a SHOW_ERRORS flag to true.

#### GitHub Actions

```yml
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    - name: Set up Python 3.9
      uses: actions/setup-python@v2
      with:
        ruby-version: 3.9
    - name: Run Pur
      env:
        SHOW_ERRORS: true
      run: wget --quiet -O - https://raw.githubusercontent.com/CICDToolbox/pur/master/pipeline.sh | bash
```

#### Travis CI

```yml
language: python
python: 3.9
env: SHOW_ERRORS=true

script:
  - wget --quiet -O - https://raw.githubusercontent.com/CICDToolbox/pur/master/pipeline.sh | bash
```

<a name="skipping-modules"></a>
### Skipping Modules

It is possible to skip modules that need to be pinned to a specific (older) version. This is done by setting the SKIP_LIST which is a comma separated list of packages to skip.

#### GitHub Actions

```yml
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    - name: Set up Python 3.9
      uses: actions/setup-python@v2
      with:
        ruby-version: 3.9
    - name: Run Pur
      env:
        SKIP_LIST: 'pur'
      run: wget --quiet -O - https://raw.githubusercontent.com/CICDToolbox/pur/master/pipeline.sh | bash
```

#### Travis CI

```yml
language: python
python: 3.9
env: SKIP_LIST='pur'

script:
  - wget --quiet -O - https://raw.githubusercontent.com/CICDToolbox/pur/master/pipeline.sh | bash
```

<a name="dry-run"></a>
### Dry Run

If you are only interested in seeing if there were errors but you do not want the pipeline to actually fail then you can run the job in `dry run` mode.

#### GitHub Actions

```yml
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    - name: Set up Python 3.9
      uses: actions/setup-python@v2
      with:
        ruby-version: 3.9
    - name: Run Pur
      env:
        DRY_RUN: true
      run: wget --quiet -O - https://raw.githubusercontent.com/CICDToolbox/pur/master/pipeline.sh | bash
```

#### Travis CI

```yml
language: python
python: 3.9
env: DRY_RUN=true

script:
  - wget --quiet -O - https://raw.githubusercontent.com/CICDToolbox/pur/master/pipeline.sh | bash
```

<a name="multiple-options"></a>
### Multiple Options

You can enable multiple options at the same time simply by setting the require environment variables on a specific job.

<a name="example-output"></a>
## Example Output

This is an example of the output report generated by this tool, this is the actual output from the tool running against itself.
```
--------------------------------------------------------------------------------
Scanning all requirements.txt with pur (version: 5.4.1)
--------------------------------------------------------------------------------
[  OK  ] Processing successful for tests/requirements.txt
--------------------------------------------------------------------------------
Total: 1, OK: 1, Failed: 0, Skipped: 0
--------------------------------------------------------------------------------
```

<a name="file-identification"></a>
## File Identification

Files are identified using the following code:

```shell
[[ ${filename} =~ \requirements.txt$ ]]
```
> There is not magic type for requirements.txt files so file -b is of not use for identifying the files.

<a name="show-support"></a>
## Show Support

<p>
<a href="https://ko-fi.com/wolfsoftware">
<img src="https://img.shields.io/badge/Ko%20Fi-blue?style=for-the-badge&logo=ko-fi&logoColor=white" />
</a>
</p>
