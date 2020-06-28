Options
---

Shell options modify how the shell environment works.
They are sweeping, and useful for debugging, or runtime modifications.

# seeing current options

The builtin variable `$-` will list the current options which are enabled.

These are of course not all options, but they sufice for managing some details.

# procedurally disabling, and re-enabling options.

```bash
# do not actually use this
# it will break a lot of your expectations
# this is for demostration purposes
function clear_all_options() {
  declare -a regexes=(
                      "*a*" "*b*" "*e*" "*f*" "*h*" "*k*" "*m*" "*n*" "*p*"
                      "*t*" "*u*" "*v*" "*x*" "*B*" "*C*" "*E*" "*H*" "*P*"
                     )
  declare -a unset=(  "+a"  "+b"  "+e"  "+f"  "+h"  "+k"  "+m"  "+n"  "+p"
                      "+t"  "+u"  "+v"  "+x"  "+B"  "+C"  "+E"  "+H"  "+P"
                     )
  for ((i = 0 ; i < ${#regexes[@]} ; i++)); do
    if [[ "$-" =~ ${regexes[$i]} ]]; then
      set "${unset[$i]}"
    fi
  done
}
```

# actual useful example

This example just handles `e`, `x`, and `v` which can leak PII, or make systems
which depend on non-zero return codes

```bash
# this function will take any number of args
# and it will execute them without `e`, `x`, `v`
# then store those options,
# and return what ever `rc` the orginal command
# would've used.
function exec_without_debugging() {

  # capture all args
  local -a ARGS=()
  for arg in "$@"; do
    ARGS+=( "${arg}" )
  done

  # nothing to do no args passed
  if [[ ${#ARGS[@]} -le 0 ]]; then
    return 0;
  fi

  # save old shell args
  local old_args="$-"

  # arrays for removing & redeclaring args
  declare -a regexes=( "*e*" "*x*" "*v*" )
  declare -a unset=( "+e"  "+x"  "+v" )
  declare -a set=( "-e" -x" "-v")

  # save the return code
  local -i rc=0

  # clear the environment
  for (( i = 0 ; i < 3 ; i++)); do
    if [[ "${old_args}" =~ ${regexes[$i]} ]]; then
      set "${unset[$i]}"
    fi
  done

  # run the command
  # in the same process
  # using the existing IO
  exec "${ARGS[@]}"
  rc=$?

  # restore the environment
  for (( i = 0; i < 3; i++)); do
    if [[ "${old_args}" =~ ${regexes[$i]} ]]; then
      set "${set[$i]}"
    fi
  done

  return $? 
}
```
