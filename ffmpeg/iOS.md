iOS
---

Apple is very picky about supported codec's & formats.

### Supported Codec's and options

Best Options

* `h264`, at level 3.1 or lower, with yuv420p color space
* `hevc`/`h265`, at level 4.1 or lower, with yuv420p color space

### Re-Encoding with hevc_amf

This uses AMD GPU acceleration.

Have up to date drivers.

```bash
#!/bin/bash
# 
# script [input_file] [output_file]
#
# Will error if either exist

fn main() {
    if [[ -r "${1}" ]]; then
        echo "cannot read input file ${1}" >&2;
	return 1;
    fi
    if [[ -f "${2}" ]]; then
    	echo "cowardly refusing to overwrite output ${2}" >&2;
	return 1;
    fi

    local -a args=(
      '-hide_banner'
      '-i' "${1}"
      '-c:v' 'hevc_amf'
      '-quality:v' '2'
      '-usage:v' '0'
      # how you say level=4.1 to hevc_amf
      '-level:v' '123'
      # apple devices WILL NOT play the files
      # without, IDFK why
      '-tag:v' 'hvc1'
      '-pix_fmt' 'yuv420p'
      '-c:a' 'aac'
      '-movflags' '+faststart'

      "${2}"
    )

    ffmpeg "${args[@]}"
}

main "$@"
```

### Re-encoding h264_amf

Encoding for h264 on AMD GPU

This will _somewhat_ reduce quality on extremely high fidelity files

```bash
#!/bin/bash
# 
# script [input_file] [output_file]
#
# Will error if either exist

fn main() {
    if [[ -r "${1}" ]]; then
        echo "cannot read input file ${1}" >&2;
	return 1;
    fi
    if [[ -f "${2}" ]]; then
    	echo "cowardly refusing to overwrite output ${2}" >&2;
	return 1;
    fi

    local -a args=(
      '-hide_banner'
      '-i' "${1}"
      '-crf' '0'
      '-c:v' 'h264_amf'
      '-quality:v' '2'
      '-usage:v' '0'
      # how you say level=4.1 to hevc_amf
      '-level:v' '30'
      '-pix_fmt' 'yuv420p'
      '-c:a' 'aac'
      '-movflags' '+faststart'
      "${2}"
    )

    ffmpeg "${args[@]}"
}

main "$@"
```
