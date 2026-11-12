# fix_time_and_coords.py

Fix time and spatial coordinates in WIEMIP processed NetCDF outputs.

This script does two things for each selected `.nc` file:

1. Rebuilds `time` values using known WIEMIP length rules.
2. Replaces `X`, `Y`, `lat`, and `lon` using a trusted `run-mask2.nc`.

It writes fixed files to a new output location (or subfolder), preserving filenames.

## Requirements

- Python 3.9+
- Python packages:
  - `netCDF4`
  - `numpy`

Install:

```bash
pip install netCDF4 numpy
```

## Quick Start

```bash
python fix_time_and_coords.py <input-root> [output-root] --run-mask /path/to/run-mask2.nc
```

- `<input-root>`: directory containing case folders
- `[output-root]`: optional; defaults to the script directory

## Common Usage Patterns

### 1) Process all `.nc` files in all cases

```bash
python fix_time_and_coords.py \
  /path/to/ProcessedOutputRepeat \
  /path/to/output_root \
  --case-input-subdir filtered_wiemip_output \
  --case-output-subdir filtered_wiemip_output_fixed \
  --run-mask /path/to/run-mask2.nc \
  --overwrite
```

### 2) Process only selected variables (`soilT`, `mrsoLayer`)

```bash
python fix_time_and_coords.py \
  /path/to/ProcessedOutputRepeat \
  /path/to/output_root \
  --case-input-subdir filtered_wiemip_output \
  --case-output-subdir filtered_wiemip_output_fixed \
  --file-patterns "*soilT*.nc" "*mrsoLayer*.nc" \
  --run-mask /path/to/run-mask2.nc \
  --overwrite
```

### 3) Write to a central fixed tree (`<aggregate>/<case>/*.nc`)

```bash
python fix_time_and_coords.py \
  /path/to/ProcessedOutputRepeat \
  --case-input-subdir filtered_wiemip_output \
  --aggregate-output-root /path/to/ProcessedOutputRepeat_fixed \
  --run-mask /path/to/run-mask2.nc \
  --overwrite
```

## Important Flags

- `--cases <case1> <case2> ...`  
  Process only specific case folders.
- `--file-patterns "<glob1>" "<glob2>" ...`  
  Only process matching files (example: `*soilT*.nc`).
- `--dry-run`  
  Print what would be processed without writing output.
- `--overwrite`  
  Replace outputs if they already exist.

## Input Folder Expectations

By default, the script expects case folders under `<input-root>`.

If files are inside a subfolder per case (common pattern):

`<input-root>/<case>/filtered_wiemip_output/*.nc`

then use:

`--case-input-subdir filtered_wiemip_output`

## Output Layout

Default output path:

`<output-root>/ProcessedOutput_fixed/<case>/<file>.nc`

If `--case-output-subdir` is set, output goes to:

`<input-root>/<case>/<case-output-subdir>/<file>.nc`

If `--aggregate-output-root` is set, output goes to:

`<aggregate-output-root>/<case>/<file>.nc`

## Help

```bash
python fix_time_and_coords.py --help
```

