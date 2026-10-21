# QGIS practice project: Prairie provinces within Canada

A first learning project in QGIS. I loaded provincial boundaries, set a Canadian coordinate system, joined a small attribute table, fixed a name mismatch, calculated areas and produced a map. This is a practice exercise and not published research.

## Data

- Boundaries: Natural Earth 1:10m Cultural Vectors, Admin 1 States and Provinces (`ne_10m_admin_1_states_provinces.zip`, version 5.1.1).
- Downloaded on 20 October 2026 from https://www.naturalearthdata.com/downloads/10m-cultural-vectors/
- Region table: `regions.csv`, which I wrote myself. It labels Alberta, Saskatchewan and Manitoba as "Prairie" and every other province and territory as "Other".

Natural Earth shows de facto boundaries and the data is generalised for small scale mapping, so the areas below are approximate and are not official surveyed figures.

## Software

QGIS (version: add yours from Help > About), run on Ubuntu Linux.

## Method

1. Loaded the Natural Earth layer. In the attribute table the province name field is `name` and the country field is `admin`.
2. Set the project coordinate system to NAD83 / Canada Atlas Lambert (EPSG:3978).
3. Selected the Canadian features with the expression `"admin" = 'Canada'` (13 provinces and territories) and exported them to a GeoPackage in EPSG:3978.
4. Loaded `regions.csv` as an attribute only table and joined it to the Canadian layer on the province name.
5. Data quality check: the join left one province without a region value. The cause was a spelling mismatch for Quebec, because the boundary layer uses "Québec" with an accent and my table used "Quebec". I corrected the name in the table to "Québec", removed the table layer in QGIS and loaded it again. All 13 features then matched.
6. Calculated area in square kilometres with the field calculator, using the expression `$area / 1000000` on the layer stored in EPSG:3978 (units: meters). Project ellipsoid setting used: add the name shown in Project > Properties > General.
7. Totalled the areas with the Statistics panel, for all of Canada and for the selected Prairie provinces.
8. Styled the map by region and exported a PDF through a print layout.

### Error found and corrected

My first area values were far too small. The extent of my exported layer still showed longitude and latitude values even though the layer was labelled EPSG:3978, so the stored coordinates had not been converted to the projection. I found this by checking the layer's extent in the Information tab and by noticing that Quebec's area was only about 204 (square degrees). I set the label back to EPSG:4326, exported the layer again with the CRS set to EPSG:3978, confirmed the extent was now in meters, and recalculated. A layer's label and its stored coordinates are separate, and only an export with a new CRS converts them.

## Results

- Total area of Canada in this layer: about 9.95 million km²
- Area of Alberta, Saskatchewan and Manitoba together: about 1.97 million km²
- Prairie share of Canada's area in this layer: about 19.8%

Sanity check: Quebec's area in the layer is about 1.47 million km², against a published total of about 1.54 million km² (Statistics Canada, to be verified). Canada's total here is within about 0.4% of the commonly quoted 9.98 million km², and the Prairie total is within about 0.3% of the sum of the three provinces' published total areas. The areas are calculated by QGIS from generalised data, so they are approximate.

## Limitations

- First QGIS project, so this shows basic skills only: loading data, coordinate systems, joins, area calculation and map layout.
- The data is generalised, so areas differ slightly from official figures.
- I have not used ArcGIS Pro. The concepts of layers, coordinate systems and joins are the same.

## Files

- `qgis_first_project.qgz`: QGIS project file
- The exported provinces layer, a GeoPackage stored in EPSG:3978 (file name as saved on my computer)
- `regions.csv`: region table used for the join
- `Prairie_map.pdf`: exported map

The Natural Earth download is not included. Download it from the link above.

## Author

Adebayo A. Adeniyi, October 2026
