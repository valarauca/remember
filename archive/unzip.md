unzip
---

# List Files

basic listing:

```bash
unzip -l "${zip_archive}"
```

verbose listing:

```bash
unzip -v "${zip_archive}"
```

# Unzip to directory

```bash
unzip -d "${directory_to_populate}" "${zip_archive}"
```
