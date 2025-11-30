# Initial notes

First pass at variable bit rate

## script

```bash
#!/bin/bash
FFMPEG_BIN="ffmpeg.exe"

function run_ffmpeg() {
    local qvbr="${1}"

    local -a args=(
        '-i' "${2}"
        '-c:v' 'av1_amf'
        '-quality' 'high_quality'
        '-usage' 'high_quality'
        '-filler_data' 'false'
        '-rc' 'qvbr'
        '-qvbr_quality_level' "$qvbr"
        '-b:v' '0'
        '-max_b_frames' '3'
        '-header_insertion_mode' 'gop'
        '-preanalysis' '1'
        '-pa_lookahead_buffer_depth' '35'
        '-pa_scene_change_detection_enable' '1'
        '-vbaq' 'true'
        '-pa_adaptive_mini_gop' '1'
        '-aq_mode' '1'
        "out(qvbr=${qvbr}).mkv"
    )
    "${FFMPEG_BIN}" "${args[@]}"
}

run_ffmpeg "${1}" "${2}"
```

### qvbr=51

file size 143MiB (increase from 82MiB)

```
[Parsed_ssim_0 @ 000001cbe09f0c40] SSIM Y:0.994148 (22.326609) U:0.995281 (23.261207) V:0.995345 (23.321132) All:0.994536 (22.624914)
[Parsed_psnr_1 @ 000001cbe09f2340] PSNR y:49.459688 u:52.302034 v:52.541599 average:50.241490 min:48.290019 max:inf
```

### qvbr=40

file size 105MiB (increase from 82MiB)

```
[Parsed_ssim_0 @ 000001ddce7ecac0] SSIM Y:0.992412 (21.198494) U:0.994244 (22.398854) V:0.994321 (22.456970) All:0.993035 (21.570897)
[Parsed_psnr_1 @ 000001ddce7ed9c0] PSNR y:47.858214 u:51.068349 v:51.408100 average:48.720873 min:46.613545 max:inf
```

### qvbr=30

file size 72MiB (decrease from 82MiB)

```
[Parsed_ssim_0 @ 000001bc61181c40] SSIM Y:0.989907 (19.959799) U:0.992950 (21.518301) V:0.993057 (21.584418) All:0.990939 (20.428331)
[Parsed_psnr_1 @ 000001bc61181140] PSNR y:46.285736 u:49.784060 v:50.198086 average:47.207074 min:45.136038 max:inf
```

### qvbr=25

file size is 58.8MiB

```
[Parsed_ssim_0 @ 000002633a771840] SSIM Y:0.988019 (19.215222) U:0.992202 (21.080135) V:0.992298 (21.133921) All:0.989430 (19.759082)
[Parsed_psnr_1 @ 000002633a771f40] PSNR y:45.310303 u:49.118360 v:49.538342 average:46.284980 min:44.159197 max:inf
```

### qvbr=20

file size 45MiB (decrease from 82MiB)

```
[Parsed_ssim_0 @ 000002ee9b7f1a40] SSIM Y:0.985326 (18.334479) U:0.991060 (20.486567) V:0.991219 (20.564354) All:0.987264 (18.949555)
[Parsed_psnr_1 @ 000002ee9b7f1c40] PSNR y:44.158946 u:48.221765 v:48.673758 average:45.177185 min:43.062950 max:inf
```

### qvbr=15

file size 34MiB (decrease from 82MiB)

```
[Parsed_ssim_0 @ 00000170c3f49940] SSIM Y:0.981386 (17.301571) U:0.989769 (19.900828) V:0.989869 (19.943362) All:0.984197 (18.012568)
[Parsed_psnr_1 @ 00000170c3f48340] PSNR y:42.907246 u:47.239940 v:47.713769 average:43.968350 min:41.952357 max:inf
```

### qvbr=10

file size 26MiB (decrease from 82MiB)

```
[Parsed_ssim_0 @ 0000013b3ce883c0] SSIM Y:0.976172 (16.229152) U:0.988229 (19.291964) V:0.988390 (19.351734) All:0.980218 (17.037299)
[Parsed_psnr_1 @ 0000013b3ce87bc0] PSNR y:41.634232 u:46.219207 v:46.705190 average:42.732778 min:40.859802 max:inf
```

### qvbr=5

file size is 20MiB (decrease form 82MiB)

```
[Parsed_ssim_0 @ 000001c09ca18140] SSIM Y:0.969660 (15.179847) U:0.986494 (18.694847) V:0.986668 (18.751166) All:0.975300 (16.073112)
[Parsed_psnr_1 @ 000001c09ca17340] PSNR y:40.396947 u:45.206386 v:45.691841 average:41.526516 min:39.607833 max:inf
```
