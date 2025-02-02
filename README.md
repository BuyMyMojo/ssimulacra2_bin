# Another ssimulacra2 cli

[![Building](https://github.com/BuyMyMojo/ssimulacra2_bin/actions/workflows/rust.yml/badge.svg)](https://github.com/BuyMyMojo/ssimulacra2_bin/actions/workflows/rust.yml)

A fork of [ssimulacra2_bin](https://github.com/rust-av/ssimulacra2_bin) that I could not stop adding things to. *whoops*

If you want **full ffmpeg based video support** check out their version. My version is based on folders of images and full windows compatability.

CLI for the Rust implementation of SSIMULACRA2: <https://github.com/rust-av/ssimulacra2>

## Download

Get the release version from [here](https://github.com/BuyMyMojo/another_ssimulacra2_cli/releases)

Get the requivalent of a nightly build from the [build action](https://github.com/BuyMyMojo/ssimulacra2_bin/actions/workflows/rust.yml)

## Install

### Cargo Crates.io

```bash
cargo install as2c
```

### Cargo Git

```bash
cargo install as2c --git https://github.com/BuyMyMojo/another_ssimulacra2_cli
```

then you can just run `as2c` from your terminal.

## usage

[Supported image formats](https://github.com/image-rs/image/blob/master/README.md#supported-image-formats)

```bash
Another ssimulacra2 cli

Usage: as2c[.exe] [OPTIONS] <SOURCE> <DISTORTED> [OUT]

Arguments:
  <SOURCE>     Original unmodified image
  <DISTORTED>  Distorted image
  [OUT]        Output folder or `.csv` file. Requires --folders

Options:
  -t, --threads <THREADS>
          Choose how many cpu threads to use. defaults to your core count!
  -f, --folders
          If input paths are folders, process all images in the folders. This assumes the files are named the same in both folders.
      --colour-space <COLOUR_SPACE>
          https://docs.rs/av-data/0.4.1/av_data/pixel/enum.ColorPrimaries.html for more info [default: bt709] [possible values: reserved0, bt709, unspecified, reserved, bt470m, bt470bg, st170m, st240m, film, bt2020, st428, p3dci, p3-display, tech3213]
      --colour-transfer <COLOUR_TRANSFER>
          https://docs.rs/av-data/0.4.1/av_data/pixel/enum.TransferCharacteristic.html for more info [default: srgb] [possible values: reserved0, bt1886, unspecified, reserved, bt470m, bt470bg, st170m, st240m, linear, logarithmic100, logarithmic316, xvycc, bt1361e, srgb, bt2020-ten, bt2020-twelve, perceptual-quantizer, st428, hybrid-log-gamma]
  -h, --help
          Print help information
  -V, --version
          Print version information
```

## Examples

Comparing indavidual images/frames:

```bash
as2c[.exe] ./source.png ./compressed.png
81.62440732
```

Comparing two folders of frames:

```bash
as2c[.exe] -f ./source/ ./compressed/
Min: 44.74846668101959
Max: 57.51044138134341
Mean: 51.218276319631876
```

Comparing two folders of frames and output CSV with each frame's results:

```bash
as2c[.exe] -f ./source/ ./compressed/ ./out.csv
```

or just

```bash
as2c[.exe] -f ./source/ ./compressed/ ./
```

example csv table:

| frame | ssimulacra2        |
| ----- | ------------------ |
| 0     | 49.924279448275605 |
| 1     | 53.36855269651266  |
| 2     | 56.70124266359252  |
| 3     | 54.235634583548276 |
| 4     | 56.10589175633655  |

## Colour formats

supported ColorPrimaries(`--colour-space`):

```txt
Reserved0
BT709
Unspecified
Reserved
BT470M
BT470BG
ST170M
ST240M
Film
BT2020
ST428
P3DCI
P3Display
Tech3213
```

supported TransferCharacteristic(`--colour-transfer`):

```txt
Reserved0
BT1886
Unspecified
Reserved
BT470M
BT470BG
ST170M
ST240M
Linear
Logarithmic100
Logarithmic316
XVYCC
BT1361E
SRGB
BT2020Ten
BT2020Twelve
PerceptualQuantizer
ST428
HybridLogGamma
```

## Memory usage

On my 5600x when comparing two folders of 1440p PNG files it can use 3-4GB of ram since it basically has 12 pairs of raw 1440p images in memory.

make sure to lower the threads used with `--threads` if you run out of memory.
