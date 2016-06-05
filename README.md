[![npm version](https://img.shields.io/npm/v/grp-cli.svg)](https://npmjs.com/package/grp-cli) [![license](https://img.shields.io/npm/l/grp-cli.svg)](https://github.com/mklement0/grp-cli/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [grp &mdash; Introduction](#grp-&mdash-introduction)
- [Examples](#examples)
- [Installation](#installation)
  - [From the npm registry](#from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# grp &mdash; Introduction

`grp` is a Unix CLI that facilitates breaking text into ***gr***ou***p***s of characters with a variety of options
and also offers formatting numbers with digit grouping (thousands separators) based on the active locale.

See the examples below, concise [usage information](#usage) further below, or read the [manual](doc/grp.md).

# Examples

```shell
  # By default, break a string into space-separated groups of 3 chars.,
  # starting from the right (end).
$ grp 1000000 2000
1 000 000 
2 000

  # Use proper number formatting (separation with locale-specific thousands
  # separators) with -n; example output from the U.S. English locale:
$ grp -n 1000000 1999.99
1,000,000
1,999.99
  
   # Break input into lines of 3 characters each, starting from the left:
$ grp -c 3 -s $'\n' -l abcdefgh
abc
def
gh

   # Insert a '.' between characters:
$ grp -c 1 -s . abcdef
a.b.c.d.e.f

   # Enclose each character in square brackets:
$ grp -c 1 -f '[%s]' abc
[a][b][c]

   # Format text as a US telephone number:
$ echo '6085277865' | grp -f '+1 (%s) %s-%s' -c 3,3,4 
+1 (608) 527-7865

  # Break the input into repeating groups of 2 and 1 char. each:
$ grp -l -c 2,1+ -s / abcdefgh
ab/c/de/f/gh
```

# Installation

**Supported platforms**

* When installing from the **npm registry**: **Linux** and **OSX**
* When installing **manually**: any **Unix-like** platform with **Bash** 

## From the npm registry

With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install [the package](https://www.npmjs.com/package/grp-cli) as follows:

    [sudo] npm install grp-cli -g

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `grp` in your system's `$PATH`.

## Manual installation

* Download [the CLI](https://raw.githubusercontent.com/mklement0/grp-cli/stable/bin/grp) as `grp`.
* Make it executable with `chmod +x grp`.
* Move it or symlink it to a folder in your `$PATH`, such as `/usr/local/bin` (OSX) or `/usr/bin` (Linux).

# Usage

Find concise usage information below; for complete documentation, read the [manual online](doc/grp.md) or, 
once installed, run `man grp` (`grp --man` if installed manually).

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ grp --help


Format numbers with digit grouping:

    grp -n [-t <term>] [<num>...]

Break text into groups of characters:

    grp [-l | -r] [-c <count>] [-s <sep> | -f <fmt>] [-t <term>] [<txt>...]

Options:

    -n          apply locale-aware digit grouping to input numbers
    -t <term>   terminator to append to each argument's result; default: \n
    -l, -r      start grouping from left / right (default)
    -c <count>  count of chars. per group - may be comma-separated list
    -s <sep>    either: separator to place between groups; default: a space
    -f <fmt>    or: printf-style format string to apply to groups
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015-2016 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [marked-man (D)](https://github.com/kapouer/marked-man#readme)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [tap (D)](https://github.com/isaacs/node-tap)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.1.4](https://github.com/mklement0/grp-cli/compare/v0.1.3...v0.1.4)** (2016-06-05):
  * [fix] When using `-n` to group numbers, the `-t` option is now respected.
  * [enhancement] Improved detection of invalid numbers when using `-n`. 

* **[v0.1.3](https://github.com/mklement0/grp-cli/compare/v0.1.2...v0.1.3)** (2015-09-17):
  * [doc] `grp` now comes with a man page (invoke with `grp --man` in case of manual installation); `grp -h` now just prints concise usage info.

* **[v0.1.2](https://github.com/mklement0/grp-cli/compare/v0.1.1...v0.1.2)** (2015-09-15):
  * [dev] Makefile improvements; various other behind-the-scenes tweaks.

* **[v0.1.1](https://github.com/mklement0/grp-cli/compare/v0.1.0...v0.1.1)** (2015-06-13):
  * [doc] Read-me fixed and amended.

* **v0.1.0** (2015-06-13):
  * Initial release.
