Fiona-Rasterio-Shapely Cheat Sheet
==================================

A cheat sheet for Fiona/Rasterio/Shapely command-line geodata tools, with apologies to [@dwtkns](https://github.com/dwtkns/gdal-cheat-sheet). Suggestions welcome!

Vector operations
---

#### Get Fiona version

New in 1.4.3

	fio --version

#### Enumerate format drivers

New in 1.4.3

	fio env --formats

#### Get vector information

	fio info input.shp

#### Print vector format

	fio info input.shp --format

#### Print vector extent

	fio info input.shp --bounds

#### Print vector coordinate reference system

	fio info input.shp --crs
	
#### Print count of features

	fio info input.shp --count
	
#### List vector drivers

	TODO

#### Convert vectors to GeoJSON

With coordinate reference system transformation and rounding of numbers to a constant precision.

	fio cat input.shp --dst_crs EPSG:4326 \
	| fio collect --precision 6 > output.json

#### Convert between vector formats

	fio cat input.shp | fio load --format Shapefile output.shp

#### Print count of features with attributes matching a given pattern

Using grep.

	fio cat input.shp | grep "Search Pattern" -c

#### Clip vectors by bounding box

	TODO

#### Clip one vector by another

	TODO

#### Reproject vector

	fio cat input.shp --dst_crs EPSG:4326 \
	| fio load --format Shapefile --dst_crs EPSG:4326 output.shp
	
#### Merge features in a vector file by attribute ("dissolve")

  	TODO

#### Merge vector files

  	fio cat *.shp | fio load --format Shapefile merged.shp

#### Filter a vector file

Using [jq](http://stedolan.github.io/jq/) for filtering and 
[geojsonio-cli](https://github.com/mapbox/geojsonio-cli) for display of results. Jq can't
yet handle RS-separated sequences, so use --x-json-seq-no-rs.

	fio cat input.shp --x-json-seq-no-rs \
	| jq 'select(.id=="10")' -c \
	| fio collect \
	| geojsonio

#### Filter a vector file in parallel

Using GNU Parallel

	fio cat input.shp --x-json-seq-no-rs \
	| parallel --pipe "jq -c 'select(.id==\"10\")'" \
	| fio collect \
	| geojsonio

#### Subset & filter all shapefiles in a directory

  	TODO

Raster operations
---

#### Get Rasterio version

New in 0.15

	rio --version

#### Enumerate format drivers

New in 0.15

	rio env --formats

#### Get raster information

As JSON – no more screen scraping the output of gdalinfo!

	rio info input.tif

#### Print raster format

	rio info input.tif --format

#### Print raster extent

	rio info input.tif --bounds

#### Print raster coordinate reference system

	rio info input.tif --crs
	
#### Print count of raster bands

	rio info input.tif --count

#### Get raster extent as GeoJSON

And pipe to geojsonio-cli:

	rio bounds input.tif | geojsonio

#### Concatenate or stack datasets together

New in 0.15.

	rio stack r.tif g.tif b.tif -o rgb.tif

#### Extract vectors from raster band as GeoJSON

	rio shapes input.tif --bidx 1 > output.json

#### Extract data/nodata vectors from raster as GeoJSON

	rio shapes input.tif --mask > output.json
	
#### Burn vector into raster

	TODO

(Lots of other operations TODO)
