Bash Array's
---

There are too many ways to do this wrong.

# function arguments

Here is how you perserve arguments (with whitespace)
and copy them into an array

```bash
function my_function() {
  local -a ARGS=()
  for arg in "$@"; do
    ARGS+=( "${arg}" )
  done
}
```

# array length

Determining Array Length

```bash
function my_function() {
  local -a ARGS=()
  for arg in "$@"; do
    ARGS+=( "${arg}" )
  done

  if [[ ${#ARGS[@]} -lt 1 ]]; then
    echo "less than 1 argument present"
    exit 1;
  fi
}
```

# array slicing

[see this link](https://gist.github.com/steakknife/8294792)
