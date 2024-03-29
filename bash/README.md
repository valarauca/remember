Bash
---

[GNU Manual is useful](https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameter-Expansion)

[Removing whitespace](https://web.archive.org/web/20121022051228/http://codesnippets.joyent.com/posts/show/1816)

## Table of Contents

* [Finding User Who Started Session](https://github.com/valarauca/remember/blob/master/bash/SessionTrace.md)
* [Handling Shell Options](https://github.com/valarauca/remember/blob/master/bash/Options.md)
* [Handling Shell Array](https://github.com/valarauca/remember/blob/master/bash/Array.md)
* [Handling Whitespace](https://github.com/valarauca/remember/blob/master/bash/Whitespace.md)
* [Check if something is installed](https://github.com/valarauca/remember/blob/master/bash/IsInstalled.md)
* [Suffix & Prefix](https://github.com/valarauca/remember/blob/master/bash/SuffixPrefix.md)

## Version compatibility notes

* `nameref` expansion (`${!var}`) was added in GNU-BASH v2.0
* `nameref` prefix expansion (`${!var*}` attempt to match on the prefix on variable name) was added in GNU-BASH v2.04
* `nameref` assignment & declaration (e.g.: `declare -n`) was added in GNU-BASH v4.3
* `nameref` assignment & aliasing of arrays was added in GNU-BASH v5.2
