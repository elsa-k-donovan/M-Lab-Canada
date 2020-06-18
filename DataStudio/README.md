SAVED QUERY FOR UNIFIED_DOWNLOADS:

```shell
SELECT
REPLACE(
REPLACE(
REPLACE (
REPLACE (
REPLACE (
REPLACE (
REPLACE (
  REPLACE(
  REPLACE(
    REPLACE (
    REPLACE (
  REPLACE (
    REPLACE (
       client.Geo.region, 'ON', 'Ontario'), 'AB', 'Alberta'), 'BC', 'British Columbia'),
  'QC', 'Quebec'), 
  'SK', 'Saskatchewan'),
  'NS', 'Nova Scotia'),
  'PE', 'Prince Edward Island'),
  'MB', 'Manitoba'),
  'NL', 'Newfoundland and Labrador'),
  'YT', 'Yukon'),
  'NT', 'Northwest Territories'),
  'NU', 'Nunavut'),
  'NB', 'New Brunswick')
  as province,
  a.MeanThroughputMbps,
  a.MinRTT,
  test_date,
  a.TestTime,
  client.Geo.city,
  client.Geo.postal_code,
  client.Geo.latitude,
  client.Geo.longitude,
  FROM `measurement-lab.ndt.unified_downloads` 
  WHERE client.Geo.country_name IN ('Canada') AND test_date BETWEEN PARSE_DATE('%Y%m%d', @DS_START_DATE) AND PARSE_DATE('%Y%m%d', @DS_END_DATE)
```
