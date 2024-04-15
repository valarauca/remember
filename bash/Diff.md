Diff & Patch
---

I wanted to make a note because I'll forget this


# Create a diff

```
diff -Naru [org_file] [new_file]
```

# Apply a diff with

```
#!/bin/bash

function apply_patch() {
  if [[ ! -r "${1}" ]]; then
    echo "patch file '${1}' is not readable, cannot be applied" >&2;
    return 1;
  fi
  if ! patch -Rsfp1 --dry-run < "${1}" &>/dev/null; then
    if ! patch -p1 -s < "${1}"; then
      echo "could not apply patch file '${1}'" >&2;
      return 1;
    fi
  fi
  return 0;
}
```
