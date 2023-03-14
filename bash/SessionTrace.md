Session Trace
---

Sometimes it is useful to find the user who started
a session before a command was ran was sudo.

General `$SUDO_USER` can be used to do this but it
maybe overriden, it also doesn't work for sticky bit
permissions

# Example

(requires Bash >= 4.0)

This example isn't perfect it breaks if there is
white space within a username (which is universally
banned by distros). 

If you wish to resolve that, writing this to use
uids is also valid.

```bash
function session_starter() {
  shopt -s lastpipe
  local this_pid=$$
  local org_user="$(whoami)"
  local this_user="${org_user}"
  while [[ "${this_user}" == "${org_user}" ]]; do
    ps h "-p${this_pid}" -ouser,ppid \
      | sed -e 's/\s*\([^ \t\n]\+\)\(\s\+\)\([0-9]\+\)/\1\n\3/g' \
      | mapfile -t arr
    this_user="${arr[0]}"
    this_pid="${arr[1]}"
  done

  if ! id -u "${this_user}" &>/dev/null; then
    echo "could not determine orginal user" >&2;
    return 1;
  fi
  export ORG_USER="${this_user}"
  export ORG_GROUP="$(id -gn "${this_user}")"
}
```
