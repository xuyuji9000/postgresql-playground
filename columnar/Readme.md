### Prepare columnar extension

``` bash
# OS: Ubuntu 18.04
# prepare dependency
apt install postgresql
apt install postgresql-server-dev-10
apt install protobuf-c-compiler
apt install libprotobuf-c0-dev

# prepare extension
make
make install
```

### prepare columnar table

``` SQL
-- load extension first time after install
CREATE EXTENSION cstore_fdw;

-- create server object
CREATE SERVER cstore_server FOREIGN DATA WRAPPER cstore_fdw;

-- create foreign table
CREATE FOREIGN TABLE trip(
    Dispatching_base_num VARCHAR(20),
    Pickup_date timestamp,
    Affiliated_base_num VARCHAR(20),
    locationID integer
)
SERVER cstore_server
OPTIONS(compression 'pglz');


-- load data from csv
copy trip from '/scratch/row/uber-raw-data-janjune-15.csv' DELIMITER ',' CSV;
```



### Query benchmark

`time psql columnar -c 'select Dispatching_base_num, count(*) from trip group by Dispatching_base_num;'`

| row     | columnar |
|---------|----------|
| 7.306s  | 3.260s   |

## Reference 

- [citusdata/cstore_fdw](https://github.com/citusdata/cstore_fdw)