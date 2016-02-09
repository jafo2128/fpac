# Introduction #

A note about versioning is in order. In the past development of FPAC there have been a number of different variations on versioning. Starting with version 3.27.15 and going forward, this is the method we will be trying to follow:
  * major.minor.revision

A major version change would take place when there is a significant change in functionality. This would likely break interoperation with previous major versions.

A minor version change could have significant fixes applied and/or additions (or deletions) of features that generally won’t affect the core operation of the software (but may enhance it).

A revision change will occur as minor bugs or groups of bugs are killed. These generally would be annoyances that have been resolved.

Version number may be appended by –beta or -rc(1,2,3...). Beta versions are just that, testing/development, all bets are off. The –rc’s are release candidates and should be pretty much stable and approaching a final release version. Note that when a version hits the release stage, it will drop its –rc and be released with just its major.minor. As bug fixes take place, revisions will be added to the major.minor (but you won’t see a major.minor.revision-beta or major.minor.revision-rc since those suffixes only apply to the next release in development).

Periodically, source tarballs will be made available for download. These versions likely still contain bugs, but should be operational for the most part.

If you want to be on the bleeding edge, anonymous SVN access is available to the source code repository. In there you will find branches related to the current stable release (in branches), and the mainline development version (in trunk).

The trunk version is not guaranteed to work at all. If you are lucky it should compile, use at your own risk. At some point the trunk will be broken off in to a branch and release built from it.

The branch version that corresponds to the major and minor number of the current released version should be the most up to date with the latest bug fixes applied.

If you want a release that should be guaranteed to compile and at least function, you will want to checkout from the tags folder in SVN. Those are snapshots that the released tarballs are built from.

As for binary packages, our major support will be for releasing Debian compatible packages. That is the platform (or its variants) that most of the development and production testing is done on. RPM’s will be provided on a best-effort basis.

As previously mentioned, binary packages of the associated AX.25 software is also maintained on the FPAC Project Page. As this is an unsupported fork off the maintainer’s site, it is no longer guaranteed to be compatible with the maintainer’s versions. Best effort will be made to try and be compatible with the maintainer’s CVS source tree to keep bugs in check.