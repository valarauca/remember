Modifying Unit Files
---

This is a royal pain in the ass.

Systemd wants you system to be resilent against you fucking around and breaking stuff.
So stuff is hard to edit, and you need to remember specific incantations.

### Easy SED shit

This is "the only way" I'm aware of to modify unit files
as part of a script, there maybe other ways.

```bash
#!/bin/bash

# errors if a thing doesn't exist
function ensure_thing_exists() {
  hash "${1}" &>/dev/null || {
    echo "${1} doesn't exist in your path, it is required" >&2;
    exit 1;
  };
}

# Applies a set command to a systemd unit files
#
# Args
# - 1: The unit's name
# - 2: The sed command
function modify_unit_files() {

  # validate number of args
  if [[ ( -z $1 ) || ( -z $2 ) ]]; then
    echo "requires 2 arguments" >&2;
    exit 1;
  fi

  # validate we're the correct user
  if [[ "0" != "${EUID}" ]]; then
    echo "need to be root/sudo to use this function" >&2;
    exit 1;
  fi

  # ensure tools we need to use exist
  ensure_thing_exists systemctl
  ensure_thing_exists tee
  ensure_thing_exists sed

  # using `script` to trick systemd into thinking
  # it has a real TTY to itself, as systemd won't
  # you do this otherwise.
  #
  # `-qfc` is globbing together:
  # -q: quiet, no extra output in stdout/stderr
  # -c: command, don't read a file, read the CLI
  # -f: allow output file to a symlink, hardlink, or in this case kernel handle.
  script -qfc "systemctl cat '${1}' | sed -e '${2}' | SYSTEMD_EDITOR=tee systemctl edit '${1}' --force --full" /dev/null
  # this will still print the contents of the unit file
  # within your terminal.
  #
  # nothing can done about that, AFAIK.
  #
  # Systemd's way to detecting if it is in
  # a tty is based on checking if `stdout`
  # and `stderr` are not the "blessed" standard
  # file descriptors.
  #
  # so we output a newline, b/c a unit file
  # may not contain that whitespace.
  echo ""

  # recreate symlinks & back up the new edit
  if sytemctl reenable "${1}"; then
    echo "failed to reenable unit:${1}" >&2;
    exit 1;
  fi
  # ensure systemd is aware the unit file changed
  # > didn't we just do that?
  # yeah, idfk know either
  systemctl daemon-reload || true

  if systemctl start "${1}"; then
    echo "failed to start ${1} after making changes" >&2;
    exit 1;
  fi
}
```
