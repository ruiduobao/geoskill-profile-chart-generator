# 剖面图生成器 | Profile Chart Generator

沿折线路径对 DEM 做等距重采样，用双线性内插逐点提取高程，计算累计地面距离，输出剖面图 PNG 与 CSV/JSON 采样表。采样点数由“路径长度/间隔”自动确定。

距离用等距圆柱近似（经度按参考纬度缩放）；双线性内插对线性平面结果精确。

## 核心算法

segment_lengths_m 米制段长 → resample_path 按弧长等距重采样 → bilinear_sample(from_bounds 像元中心约定) 提取高程 → 累计距离 → matplotlib 剖面图。

## 依赖

```bash
pip install numpy rasterio scipy matplotlib geopandas shapely pillow
```

## 使用方法

### 示例 1（合成数据，离线）

```bash
python geoskill-profile-chart-generator.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### 示例 2（自定义路径）

```bash
python geoskill-profile-chart-generator.py --input dem.tif --vertices "116.0,39.0" "116.5,39.8" "117.0,39.5"
```

### 示例 3（指定采样间隔）

```bash
python geoskill-profile-chart-generator.py --input dem.tif --interval 200
```

### 示例 4（固定采样点数）

```bash
python geoskill-profile-chart-generator.py --input dem.tif --samples 200
```

### 示例 5（合成对角线剖面）

```bash
python geoskill-profile-chart-generator.py --bbox 116 39 117 40 --synthetic --interval 500
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `profile.png` | PNG | 剖面图（主产物） |
| `profile.csv` | CSV | 采样表 index/distance/lon/lat/value |
| `profile.json` | JSON | 剖面结构化数据（可验证产物） |
| `profile_dem.tif` | GeoTIFF | 所用 DEM |

每次运行还会产出 `output-manifest.json`（运行清单）。

## 数据源 / Source

本地 GeoTIFF / 矢量文件；`--synthetic` 模式生成物理一致的模拟数据，完全离线。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传用户数据。

## License

MIT
