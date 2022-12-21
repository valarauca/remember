Url Decoding
---

Short script that works in most scenarios

```bash
function url_decode() {
    local input="${1}"
    if [[ -z $input ]]; then
        read input;
    fi
    if [[ -z $input ]]; then
       echo -e "no input given" >&2;
       return 1;
    fi
    printf '%b' "${input//%/\\x}"
}
```

Will read from the first command line argument or stdin
