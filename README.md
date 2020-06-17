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
$ ogr2ogr -f csv -dialect sqlite -sql "select AsGeoJSON(geometry) AS geom, * from transformed" transformed.csv transformed.shp
```


STEP 3:
> Upload the csv file to GCS
```shell
$ gsutil -m cp transformed.csv gs://census_statcan/
```

STEP 4:
> Create a dataset in BigQuery
> (To be written in the Cloud Shell on Google Cloud Platform)
```shell
$ bq mk demos
```

```shell
$ bq load --autodetect --replace demos.SOME_NAME gs://census_statcan/transformed.csv
```

STEP 5:
Add this SQL to BigQuery
```shell
CREATE OR REPLACE TABLE demos.CHOOSE_NEW_TABLE_NAME AS
SELECT * EXCEPT(geom), SAFE.ST_GeogFromGeoJson(geom) AS polygon
FROM demos.SOME_NAME
```

