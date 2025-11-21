# sentinel_2_tif

![PyPI](https://img.shields.io/pypi/v/sentinel_2_tif)
![License](https://img.shields.io/github/license/chase-kusterer/sentinel_2_tif)

`sentinel-2-tif` is a Python package that simplifies the process of generating georeferenced `.tif` images from Sentinel-2 Level-2A satellite data. It offers tools for spatial buffering, time window generation, scene compositing, and automatic `.tif` file creation using Microsoft’s Planetary Computer STAC API.

With this package, you will be able to **generate multiple `.tif` images in a single data pull**, each with different bounding box sizes and time windows. This flexibility is designed to help you efficiently explore and compare composite imagery, enabling you to identify the optimal composite for your analysis.

---

## Features

- Access Sentinel-2 L2A imagery via Planetary Computer
- Create minimum bounding rectangles (MBRs) from coordinates, a DataFrame, or a dictionary
- Generate bounding box buffers in kilometers
- Create symmetrical time windows around an event date
- Develop scene search grids with bounding box and time window combinations
- Composite scenes using `median` or `mean`
- Save georeferenced `.tif` outputs with full metadata
- Designed for both notebook and production workflows

---

## Installation

Install using pip:

```bash
pip install sentinel_2_tif
```

---

## Example Usage

```python
# imports
from sentinel_2_tif.spatial import mbr_buffer, mbr_buffers
from sentinel_2_tif.temporal import time_wrapper, time_windows
from sentinel_2_tif.search import st_grid
from sentinel_2_tif.pipeline import tif_generator
from shapely.geometry import box

# instantiating minimim bounding rectangle (MBR)
bbox = mbr(west=-123.0, south=37.0, east=-121.0, north=38.5)

# generating a grid of spatial + temporal search combinations
scene_search = st_grid(
    mbr=bbox,
    buffers=[0, 1, 5],
    wrap_sizes=[1, 2],
    event_date="2022-08-07",
    as_records=False
)

# pulling scenes and generating composite .tif(s)
summary = tif_generator(
    scene_search=scene_search,
    composite="median",
    out_dir="./Collected TIFF Files"
)
```

---

## Package Modules

- `spatial`: MBR and MBR buffer generation
- `temporal`: Time window generation to expand scene selection
- `search`: Spatial-temporal search grid generator
- `pipeline`: Scene extraction and `.tif` writer

---

## License

MIT License. See [LICENSE](LICENSE) for details.
