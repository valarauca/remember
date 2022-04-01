GPG Finger Prints of RPMS
---

`yum`/`dnf` truncates the fingerprint of a GPG/PGP key
when it reports them. 

To convert a GPG key into the format used by `yum`/`dnf`
the command is roughly

```bash
function key_to_yum_fingerprint() {
    local path="${1}"
    if [[ ! -r $path ]]; then
        echo "$path is not a readable file" >&2;
        return 1;
    fi

    gpg --with-fingerprint "${path}" | grep "Key fingerprint" | cut -d "=" -f 2 | tr -d ' ' | tr '[:upper:]'
     '[:lower:]' | grep -o '.\{16\}$'
    if [[ ${PIPESTATUS[0]} -ne 0 ]]; then
        echo "gpg could not read $path" >&2;
	return 1;
    fi
    return 0
}
```

As a note, older version of `yum`/`dnf` only return `8` characters not `16`.
