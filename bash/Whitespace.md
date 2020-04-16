Whitespace
---

One can trailing & leading whitespace with

```bash
function trim_space() {
  local var="${1}"
  # leading whitespace
  var="${var#"${var%%[![:space:]]*}"}"
  # trailing whitespace
  var="${var%"${var##*[![:space:]]}"}"
}
```
