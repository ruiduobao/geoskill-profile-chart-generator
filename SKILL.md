---
name: geoskill-profile-chart-generator
description: 'Sample elevation or imagery values along a path and produce profile charts and CSV'
---

# 剖面图生成器 | Profile Chart Generator

Resamples a DEM at equal intervals along a polyline path, extracts elevation point by point using bilinear interpolation, computes cumulative ground distance, and outputs a profile chart PNG together with CSV/JSON sample tables. The number of sample points is determined automatically as path length / interval.

Distance uses an equirectangular approximation (longitudes scaled by the reference latitude); bilinear interpolation is exact for linear planar surfaces.

## Core Algorithm / 核心算法

segment_lengths_m metric segment lengths → resample_path uniform resampling by arc length → bilinear_sample (from_bounds pixel-center convention) extracts elevation → cumulative distance → matplotlib profile chart.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy matplotlib geopandas shapely pillow
```

## Usage / 使用方法

### Example 1 (synthetic data, offline)

```bash
python geoskill-profile-chart-generator.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2 (custom path)

```bash
python geoskill-profile-chart-generator.py --input dem.tif --vertices "116.0,39.0" "116.5,39.8" "117.0,39.5"
```

### Example 3 (specified sampling interval)

```bash
python geoskill-profile-chart-generator.py --input dem.tif --interval 200
```

### Example 4 (fixed number of sample points)

```bash
python geoskill-profile-chart-generator.py --input dem.tif --samples 200
```

### Example 5 (synthetic diagonal profile)

```bash
python geoskill-profile-chart-generator.py --bbox 116 39 117 40 --synthetic --interval 500
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `profile.png` | PNG | Profile chart (main output) |
| `profile.csv` | CSV | Sample table index/distance/lon/lat/value |
| `profile.json` | JSON | Structured profile data (verifiable output) |
| `profile_dem.tif` | GeoTIFF | DEM used |

Each run also produces `output-manifest.json` (run manifest).

## Data Source / 数据源 / Source

Local GeoTIFF / vector files; `--synthetic` mode generates physically consistent simulated data, fully offline.

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is performed locally; user data is never uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
