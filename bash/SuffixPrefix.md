Suffix & Prefix
---


```
function example() {
    local foo = "${1#"prefix"}"
    local bar = "${food%"suffix"}"
    echo "${bar}"
}
```
