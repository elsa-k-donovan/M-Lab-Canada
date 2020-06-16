Using census data from Statcan.

Converting to geojson files. 

Storing large geojson files in buckets in Google Cloud Platform. 

Transfer to BigQuery to combine census files with M-Lab queries. 


STEP 1:
```shell
$ brew update
$ brew install gdal
```


STEP 2:
```shell
$ ogr2ogr -f csv -dialect sqlite -sql "select AsGeoJSON(geometry) AS geom, * from lda_000b16a_e" polygon.csv lda_000b16a_e.shp
```


STEP 3:
Upload the csv file to GCS
```shell
$ gsutil -m cp *.csv gs://census_statcan/
```


