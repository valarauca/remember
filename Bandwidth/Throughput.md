Basic overview of Speed/Bandwidth Requirements

# Unit Note:

* Gb: 1000 * 1000 * 1000 * [bits]
* Mb: 1000 * 1000 * [bits]
* GB: 1000 * 1000 * 1000 * 8
* MB: 1000 * 1000 * 8
* GiB: 1024 * 1024 * 1024 * 8
* MiB: 1024 * 1024 * 8

Why?

* Your computer usually displays speed in MiB/GiB
* Most hardware is solid as Gb or Mb


# PCIe Revision to Giga Bytes per Second (GB/s)


| Revision | 1x    | 2x    | 4x     | 8x     | 16x    |
| -------- | ----- | ----- | ------ | ------ | ------ |
| 1        | 0.25  | 0.5   | 1      | 2      | 4      |
| 2        | 0.5   | 1     | 2      | 4      | 8      |
| 3        | 0.985 | 1.97  | 3.94   | 7.88   | 15.76  |
| 4        | 1.969 | 3.938 | 7.876  | 15.752 | 31.504 |
| 5        | 3.939 | 7.878 | 15.756 | 31.512 | 63.024 |

All speeds in Gigabytes per second (1000^3 * 8 per second)

**NOTE** PCIe is full-duplex/bi-directional. Meaning you can have this bandwidth to & from the device simulatiously.


# Networking


| Ethernet Standard    | Speed (advertised) | Speed [Giga-BYTES/sec] |
| -------------------- | ------------------ |----------------------- |
| 10BASE-T             | 10Mbe              | 0.00125                |
| 100BASE-T            | 100Mbe             | 0.0125                 |
| 1000BASE-T           | 1000Mbe/1Gbe       | 0.125                  |
| 10GBASE-T            | 10Gbe              | 1.25                   |
| ???                  | 25Gbe              | 3.125                  |
| 40GBase-T            | 40Gbe              | 5                      |
| ???                  | 50Gbe              | 6.25                   |
| 100GBase-(SR4\|SR10) | 100Gbe             | 12.5                   |

# SATA

Assuming 300MiB/s per HDD (e.g.: HDD sequential read speed). This situtation isn't 'common' unless you are using a larger software raid array.

**NOTE** SATA is half-duplication/directional. Meaning the full bandwidth of the link can only be used for reading/writing. You cannot read/write at the same time. If the target device supports reading/writing at the same time, both operations are limited to 50% of the bandwidth (approximately)

This chart is useful for sizing AM1166 module adapter boards.

|          | 300MiB HDD per PCIe revision/width |       |       |        |        |
| -------- | ---------------------------------- | ----- | ----- | ------ | ------ |
| Revision | 1x                                 | 2x    | 4x    | 8x     | 16x    |
| 1        | 0.79                               | 1.59  | 3.18  | 6.36   | 12.73  |
| 2        | 1.59                               | 3.18  | 6.36  | 12.73  | 25.47  |
| 3        | 3.13                               | 6.27  | 12.54 | 25.09  | 50.19  |
| 4        | 6.27                               | 12.54 | 25.08 | 50.16  | 100.33 |
| 5        | 12.54                              | 25.08 | 50.17 | 100.35 | 200.71 |

Assuming a [NAND based SSD](https://en.wikipedia.org/wiki/Flash_memory) which can fully saturate a SATA-3 6Gbit/sec link.

Revision/Width vs 750MiB/s SSD

| Revision | 1x                                 | 2x   | 4x   | 8x    | 16x   |
| -------- | ---------------------------------- | ---- | ---- | ----- | ----- |
| 1        | 0.33                               | 0.66 | 1.33 | 2.66  | 5.33  |
| 2        | 0.66                               | 1.33 | 2.66 | 5.33  | 10.66 |
| 3        | 1.31                               | 2.62 | 5.25 | 10.5  | 21.01 |
| 4        | 2.62                               | 5.25 | 10.5 | 21    | 42    |
| 5        | 5.25                               | 10.5 | 21   | 42.01 | 84.03 |

# SAS

**NOTE** SAS is fully-duplex/bi-directional. That is to say you the written amount of bandwidth reading/writing simulatiously. 

### SAS-N to SATA-3

This is assuming SATA-3 (6Gb/s). The numbers from the previous table HDD (300MiB/s) and SSD (750MiB/s) are used.

| SAS Speeds | Speed Gb/s | Speed GiB/s | SATA HDDs per link | SATA SSDs per link |
| ---------- | ---------- | ----------- | ------------------ | ------------------ |
| SAS-1      | 3          | 0.34        | 1.13               | 0.45               |
| SAS-2      | 6          | 0.69        | 2.3                | 0.92               |
| SAS-3      | 12         | 1.39        | 4.63               | 1.85               |
| SAS-4      | 22.5       | 2.61        | 8.7                | 3.48               |
| SAS-5      | 45         | 5.23        | 17.43              | 6.97               |

You can argue "Well I would have 2x that speed because I read from that many drives while writing to that many drives" as SATA is directional & SAS is bi-directional.

This is in fact true... But if those drives are part of the same ZFS VDEV (or RAID10/5/6) group, that will never happen.

### SAS-1 links to PCIe

Both these protocols are bi-directional so this is apples-to-apples

| Revision | 1x    | 2x    | 4x    | 8x    | 16x    |
| -------- | ----- | ----- | ----- | ----- | ------ |
| 1        | 0.73  | 1.47  | 2.94  | 5.88  | 11.76  |
| 2        | 1.47  | 2.94  | 5.88  | 11.76 | 23.52  |
| 3        | 2.89  | 5.79  | 11.58 | 23.17 | 46.35  |
| 4        | 5.79  | 11.58 | 23.16 | 46.32 | 92.65  |
| 5        | 11.58 | 23.17 | 46.34 | 92.68 | 185.36 |

### SAS-2 Links to PCIe

Both these protocols are bi-directional so this is apples-to-apples

| Revision | 1x   | 2x    | 4x    | 8x    | 16x   |
| -------- | ---- | ----- | ----- | ----- | ----- |
| 1        | 0.36 | 0.72  | 1.44  | 2.89  | 5.79  |
| 2        | 0.72 | 1.44  | 2.89  | 5.79  | 11.59 |
| 3        | 1.42 | 2.85  | 5.71  | 11.42 | 22.84 |
| 4        | 2.85 | 5.7   | 11.41 | 22.82 | 45.65 |
| 5        | 5.7  | 11.41 | 22.83 | 45.66 | 91.33 |

It is easy to note here that many SAS-2 HBAs that use PCIe 2.0 x8 do generally offer 4x SAS-2 links. 

### SAS-3 Links to PCIe

| Revision | 1x   | 2x   | 4x    | 8x    | 16x   |
| -------- | ---- | ---- | ----- | ----- | ----- |
| 1        | 0.17 | 0.35 | 0.71  | 1.43  | 2.87  |
| 2        | 0.35 | 0.71 | 1.43  | 2.87  | 5.75  |
| 3        | 0.7  | 1.41 | 2.83  | 5.66  | 11.33 |
| 4        | 1.41 | 2.83 | 5.66  | 11.33 | 22.66 |
| 5        | 2.83 | 5.66 | 11.33 | 22.67 | 45.34 |

What is interesting here is there are many PCIe 3.0 x8 to 2-3x SAS-3 cards, which are basically hogging lanes as you have more and enough bandwidth to support 4-5 SAS-3 lanes.

While the PCIe4.0 x8 cards that only offer 3x SAS-3 links are really lowing balling the bandwidth and letting ~25% of the links bandwidth be wasted.

