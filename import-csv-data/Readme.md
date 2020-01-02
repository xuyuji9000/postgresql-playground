### Prepare data

``` bash
# uber ride data 2015 jan - june 
wget https://raw.githubusercontent.com/fivethirtyeight/uber-tlc-foil-response/master/uber-trip-data/uber-raw-data-janjune-15.csv.zip
```


### Prepare table 

``` sql
CREATE TABLE trip(
    Dispatching_base_num VARCHAR(20),
    Pickup_date timestamp,
    Affiliated_base_num VARCHAR(20),
    locationID integer
);
```


### Import CSV

`copy trip from '/scratch/postgresql-benchmark/tmpfile.csv' DELIMITER ',' CSV;`

### Query

`time psql trip -c 'select Dispatching_base_num, count(*) from trip group by Dispatching_base_num;'`