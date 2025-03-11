# Shebang Mangling

When building an RPM it maybe common to have a script who's shebang is in the format such as `#!/usr/bin/env sh`

This (often) convert into a more concrete path during RPM packaging (e.g.: `#!/usr/bin/env sh` -> `#!/usr/bin/sh`)

The policy that controls this (at least on Fedora/Redhat) is `/usr/lib/rpm/redhat/brp-mnagle-shebangs`. 

This file _can_ (but shouldn't) be modified to change rules concerning shebang mangling. 
