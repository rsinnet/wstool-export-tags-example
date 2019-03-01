# wstool-export-tags-example

A sample repository to demonstrate the proposed wstool export --tags option: vcstools/wstool#131

## Context

Assume our workspace was deployed with [default.rosinstall](../blob/develop/default.rosinstall) and the `.rosinstall` file has not been modified from there.

Note that in repository that `develop` has a feature that has not yet been released (the aforementioned [default.rosinstall](../blob/develop/default.rosinstall)).

Further assume we are interested in generating a release of version 0.1.0, so we've checkouted out this branch and `wstool info` thus gives the following output:
```
 Localname                  S SCM Version (Spec)          UID  (Spec)                 URI  (Spec) [http(s)://...]
 ---------                  - --- --------------          -----------                 ---------------------------
 wstool-export-tags-example V git release/0.1.0  (master) 8a044c782b79 (a1c75e5c8133) git@github.com:rsinnet/wstool-export-tags-example.git
```

## Current function

**Input**: `wstool export`<br/>
**Command**: `git rev-parse --abbrev-ref HEAD`<br/>
**Output**: `release/0.1.0`

**Input**: `wstool export --exact`<br/>
**Command**: `git rev-parse HEAD`<br/>
**Output**: 8a044c782b79d0b7b251fcbc204d06e18f0ec851

**Input**: `wstool export --spec`<br/>
**Source**: _`master` read directly from `.rosinstall`_<br/>
**Output**: `master`

**Input**: `wstool export --spec --exact`<br/>
**Command**: `git rev-parse master` (_`master` read directly from `.rosinstall`_)<br/>
**Output**: `a1c75e5c81338a91f11e38e1ebc8343e382ea0fe`

## Proposed new behavior

In the following examples, the command falls back to the behavior without the `--tags`:

**Input**: `wstool export --tags`<br/>
**Command**: `git describe --tags --abbrev=0 HEAD`<br/>
**Output**: `0.1.0` (falls back on `wstool export if no tag found)

**Input**: `wstool export --tags --exact`<br/>
**Command**: `git describe --tags --abbrev=0 HEAD`<br/>
**Output**: `0.1.0` (falls back on `wstool export --exact` if no tag found)

**Input**: `wstool export --tags --spec`<br/>
**Command**: `git describe --tags --abbrev=0 master` (_`master` read directly from `.rosinstall`_)<br/>
**Output**: `0.1.0` (falls back on `wstool export --spec` if no tag found)

**Input**: `wstool export --tags --spec`<br/>
**Command**: `git describe --tags --abbrev=0 master` (_`master` read directly from `.rosinstall`_)<br/>
**Output**: `0.1.0` (falls back on `wstool export --spec --exact` if no tag found)
