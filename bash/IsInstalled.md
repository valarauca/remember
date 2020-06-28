Is Installed
---

How do you check if something is installed?

Or at least that is in `$PATH` and callable?

### Check if something is installed

Here is a basic snippet to determine if something is installed

```bash
#!/bin/bash

# errors if a thing doesn't exist
function ensure_thing_exists() {
  hash "${1}" &>/dev/null || {
    echo "${1} doesn't exist in your path, it is required" >&2;
    exit 1;
  };
}
```

`hash` is a builtin, and the `||` will be handled ever under `set -e`.
