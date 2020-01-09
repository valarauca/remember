tar
---

# create archive

Without compression:

```shell
tar cf "${new_archive_path}" "${targets[*]}"
```

Create a gzip (compressed) archive:

```shell
tar czf "${new_archive_path}" "${targets[*]}"
```

Create a xz (compressed) archive:

```shell
tar cJf "${new_archive_path}" "${targets[*]}"
```

Create a bzip2 (compressed) archive:

```shell
tar cjf "${new_archive_path}" "${targets[*]}"
```

# extract files

Tar will automatically detect the compression
used and provided it is builtin to the tar
you are using, automatically handle decompression
and extraction.

```shell
tar xf "${archive_path}"
```

If you want to extract files to a specific location
the best option is to:

```shell
tar -C "${directory_to_popuate}" -xf "${archive_path}"
```
