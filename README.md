Using census data from Statcan.

Converting to geojson files. 

Storing large geojson files in buckets in Google Cloud Platform. 

Transfer to BigQuery to combine census files with M-Lab queries. 

## Add Census Data to BigQuery Viz
- [Statistics Canada](https://www150.statcan.gc.ca/n1/en/type/data)

STEP 1:
```shell
$ brew update
$ brew install gdal
```


STEP 2:
> Coordinates from StatCan files are in ESRI format. The coordinate system needs to be converted to (lat, long) in order to be used in Big Query Geo Viz.

```shell
$ ogr2ogr -t_srs EPSG:4326 -f “ESRI Shapefile” transformed.shp original.shp
```


```shell
$ ogr2ogr -f csv -dialect sqlite -sql "select AsGeoJSON(geometry) AS geom, * from lda_000b16a_e" polygon.csv lda_000b16a_e.shp
```


STEP 3:
> Upload the csv file to GCS
```shell
$ gsutil -m cp *.csv gs://census_statcan/
```


