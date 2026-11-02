# Profile Chart Generator (geoskill-profile-chart-generator)

> Sample elevation or imagery values along a path and produce profile charts and CSV

---

## 1. Overview

Resamples the DEM at equal spacing along a polyline path, extracts elevation point-by-point with bilinear interpolation, computes cumulative ground distance, and outputs a profile-chart PNG plus CSV/JSON sampling tables. The number of sample points is determined automatically from path length / interval. Distances use an equidistant-cylindrical approximation (longitude scaled by the reference latitude); bilinear interpolation is exact for linear planar results.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-profile-chart-generator.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `profile.png` | PNG | Profile chart (primary output) |
| `profile.csv` | CSV | Sampling table index/distance/lon/lat/value |
| `profile.json` | JSON | Structured profile data (verifiable output) |
| `profile_dem.tif` | GeoTIFF | The DEM used |

Each run also produces `output-manifest.json` (run manifest).


## 6. Technical Principle

segment_lengths_m (metric segment lengths) → resample_path (equal-spacing resampling along arc length) → bilinear_sample (from_bounds pixel-center convention) extracts elevation → cumulative distance → matplotlib profile chart.

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
