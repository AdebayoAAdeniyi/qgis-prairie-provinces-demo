# QGIS practice project: Prairie provinces within Canada

A first learning project in QGIS. I loaded provincial boundaries, set a Canadian coordinate system, joined a small attribute table, fixed a name mismatch, calculated areas and produced a map. This is a practice exercise and not published research.

## Data

- Boundaries: Natural Earth 1:10m Cultural Vectors, Admin 1 States and Provinces (`ne_10m_admin_1_states_provinces.zip`, version 5.1.1).
- Downloaded on 20 October 2026 from https://www.naturalearthdata.com/downloads/10m-cultural-vectors/
- Region table: `regions.csv`, which I wrote myself. It labels Alberta, Saskatchewan and Manitoba as "Prairie" and every other province and territory as "Other".
- Published areas used for comparison: Statistics Canada, Canada Year Book 2012, Table 15.6, Land and freshwater area, by province and territory (https://www150.statcan.gc.ca/n1/pub/11-402-x/2012000/chap/geo/tbl/tbl06-eng.htm).

Natural Earth shows de facto boundaries and the data is generalised for small scale mapping, so the areas below are approximate and are not official surveyed figures.

## Software

QGIS 3.44.7 Solothurn, run on Ubuntu Linux.

## Method

1. Loaded the Natural Earth layer. In the attribute table the province name field is `name` and the country field is `admin`.
2. Set the project coordinate system to NAD83 / Canada Atlas Lambert (EPSG:3978).
3. Selected the Canadian features with the expression `"admin" = 'Canada'` (13 provinces and territories) and exported them to a GeoPackage, `canada_provinces.gpkg`, in EPSG:3978.
4. Loaded `regions.csv` as an attribute only table and joined it to the Canadian layer on the province name.
5. Data quality check: the join left one province without a region value. The cause was a spelling mismatch for Quebec, because the boundary layer uses "Québec" with an accent and my table used "Quebec". I corrected the name in the table to "Québec", removed the table layer in QGIS and loaded it again. All 13 features then matched.
6. Calculated area in square kilometres with the field calculator, using the expression `$area / 1000000`. Project ellipsoid setting used: GRS 1980 (EPSG:7019).
7. Totalled the areas with the Statistics panel, for all of Canada and for the selected Prairie provinces.
8. Styled the map by region and exported a PDF through a print layout.

### Error found and corrected

My first area values were far too small. The extent of my exported layer still showed longitude and latitude values even though the layer was labelled EPSG:3978, so the stored coordinates had not been converted to the projection. I found this by checking the layer's extent in the Information tab and by noticing that Quebec's area was only about 204 (square degrees). I set the label back to EPSG:4326, exported the layer again with the CRS set to EPSG:3978, confirmed the extent was now in meters, and recalculated. A layer's label and its stored coordinates are separate, and only an export with a new CRS converts them.

## Results

| | Layer (QGIS) | Published (Statistics Canada) | Difference |
|---|---|---|---|
| Canada | about 9.95 million km² | 9,984,670 km² | about 0.3% lower |
| Alberta, Saskatchewan and Manitoba together | about 1.97 million km² | 1,960,681 km² | about 0.5% higher |
| Quebec | about 1.51 million km² | 1,542,056 km² | about 2% lower |

The Prairie share of Canada's area is about 19.8% in this layer and about 19.6% using the published figures.

The published Prairie figure is the sum of Alberta (661,848 km²), Saskatchewan (651,036 km²) and Manitoba (647,797 km²). Differences are calculated from my rounded layer values, so they are approximate. The layer areas come from generalised data, and I have not investigated why Quebec differs more than the other results.

## Limitations

- First QGIS project, so this shows basic skills only: loading data, coordinate systems, joins, area calculation and map layout.
- The data is generalised, so areas differ slightly from official figures.
- I have not used ArcGIS Pro. The concepts of layers, coordinate systems and joins are the same.

## Files

- `qgis_first_project.qgz`: QGIS project file
- `canada_provinces.gpkg`: exported provinces layer, stored in EPSG:3978
- `regions.csv`: region table used for the join
- `Prairie_map.pdf`: exported map

The Natural Earth download is not included. Download it from the link above.

## Author

Adebayo A. Adeniyi, October 2026
