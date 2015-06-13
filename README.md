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

See the examples below and the [Usage](#usage) chapter for details.

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

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```
$ grp --help

SYNOPSIS
  grp [-l | -r] [-c count] [-s sep | -f fmt] [-t term] [txt ...]
  grp -n [-t term] [num ...]

DESCRIPTION
  grp facilitates breaking input into *gr*ou*p*s:

  First synopsis form:
    Breaks text into groups of specifiable length joined with a 
    specifiable separator or formatted with a printf-style format string.
  
  Second synopsis form:
    Prints numbers with digit grouping (thousands separators), as defined by
    the current locale.

  Input is taken either from operands or, in their absence, line by line from
  stdin. Each operand / input line is processed separately.

OPTIONS

  -l, -r (default: -r)
    Determines if grouping should start from the left (-l) or right (-r) of
    the input.

  -c count
  -c count1,count2,...
  -c count1,count2,...[+]
    The count of characters to put into each group.
    - If only a single count is specified:
      Breaks the input into fixed groups of the specified size until the input
      runs out; starting either from the left (start) or right (end), as
      controlled with -l or -r.
    - With a list of counts:
      The order in which the counts are applied is implied by -l or -r; for 
      instance, since grouping starts from the right (end) by default, the
      *last* count in the list is by default applied first to the end of the
      input.
      - If the list ends in '+':
        The list of counts is applied *cyclically*; that is, once all counts
        have been applied, the process starts over with the remaining input.
      - Otherwise:
        Whatever is left of the input is put into a final group.

  -s sep (default: a space)
    The separator character or string to place between resulting groups.

  -f fmt
    The printf-style format string to apply to the resulting groups, as an 
    alternative to specifying a fixed separator.
    Note that if there are more groups than arguments in the format string,
    the format string is applied cyclically.

  -t term (default: a newline)
    The terminator char. or string to append to each group on output;
    specify -t '' to directly concatenate multiple results, if applicable,
    and to not terminate the overall input with a newline.

  -n
    Treats each input argument as a number to be format with digit grouping
    (thousands separators), as defined by the current locale.
    Input can be decimal integers or fractions, numbers in decimal
    scientific notation, or hexadecimal integers.
    Output is always decimal, with the number of input decimal places
    preserved, and the locale's digit grouping and decimal mark applied, if
    applicable,

    Note:
      - Environment variables LC_NUMERIC and, indirectly, LANG or LC_ALL
        control the active locale with respect to number formatting. Not all
        locales define digit grouping, notably not the "C" and "POSIX"
        locales. However, even the definition of real-world locales such as
        "de_DE.UTF-8" (Germany) is inconsistent across platforms, and uses
        digit grouping on some (e.g., Linux), but not others (e.g., OSX).
      - Since round-trip number conversion is involved, rounding errors can be
        introduced. To be safe, limit numbers to 15 significant digits.

EXAMPLES
  grp 1000000 # -> '1 000 000'
  grp -n 1000000.2 # -> '1,000,000.2' in locale 'en_US.UTF-8'
    # Format a US telephone number:
  grp -f '(%s) %s-%s' -c 3,3,4 '6085277865' # -> '(608) 527-7865'
    # Enclose each group in delimiters:
  grp -c 1 -f '[%s]' abc  # -> '[a][b][c]'
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [tap (D)](https://github.com/isaacs/node-tap)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.1.1](https://github.com/mklement0/grp-cli/compare/v0.1.0...v0.1.1)** (2015-06-13):
  * [doc] Read-me fixed and amended.

* **v0.1.0** (2015-06-13):
  * Initial release.
