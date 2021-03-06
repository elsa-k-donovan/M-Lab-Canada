## Add Census Data to BigQuery
- Download the data from [Statistics Canada](https://www150.statcan.gc.ca/n1/en/type/data)

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
> Add this SQL to BigQuery
```shell
CREATE OR REPLACE TABLE demos.CHOOSE_NEW_TABLE_NAME AS
SELECT * EXCEPT(geom), SAFE.ST_GeogFromGeoJson(geom) AS polygon
FROM demos.SOME_NAME
```
## Visualize Census Data on BigQuery Geo Viz
> Open BigQuery Geo Viz: https://bigquerygeoviz.appspot.com/

> Standard SQL:
```shell
SELECT polygon as district

FROM censusstatcan.demos.electoraldistricts_2016
WHERE PRUID=10
```

  ![alt-text](https://github.com/elsa-k-donovan/M-Lab-Canada/blob/master/census_example.png)

## Combine Census Data with M-Lab Data on BigQuery Geo Viz

```shell
SELECT polygon as zipcode
FROM censusstatcan.demos.disseminationareas_2016
WHERE CDNAME ='Sherbrooke'
```

> Join Canada Census and M-Lab Data ///*IN PROGRESS*///
```shell
CREATE OR REPLACE TABLE censusstatcan.demos.federal_internet AS

SELECT 
a.MeanThroughputMbps as downloadSpeed, 
a.MinRTT,
REPLACE(client.Geo.region, 'ON', 'Ontario') as province,
FROM measurement-lab.ndt.unified_downloads as x

join (SELECT * FROM censusstatcan.demos.electoraldistricts_2016) b

ON x.client.Geo.region = b.PRNAME

WHERE b.PRNAME ="Ontario"
```
