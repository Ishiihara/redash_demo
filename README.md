# TiDB Demo Redash
## Redash
Deploy Redash on your local machine with docker-compose

```
$ docker-compose -f docker-compose-prod.yaml up -d
```

After the deployment, you can use `docker ps` to check the status of the deployment.

Before accessing the Redash UI, please create a database:
```
$ docker-compose run --rm server create_db
```

To destroy, use
```
$ docker-compose down
```

You can access the Redash UI using `http:localhost:5000`. Please follow the instructions for intial setup. 

## TiDB
You can use TiUP to deploy TiDB. Be sure to include one or more TiFlash nodes. 


## Add Data Source
You can add data sources to Redash by following the getting started guide after intial setup. One thing to note is that since Redash is deployed docker containers and to access the local host, please use `host.docker.internal` instead.

## Testing
We would like to import the data intially with TiDB Lightining local backend and then simulate data insert using tidb-lightning with TiDB backend. 

Before running TiDB Lightning, please create table by executing the SQL query in `create_table.sql`

We can run some queries in this [blog post](https://pingcap.com/blog-cn/tidb-and-tiflash-vs-mysql-mariadb-greenplum-apache-spark/). Run `EXPLAIN` and `EXPLAIN ANALYZE` to inspect the query plans. 

After running some queries, we can add a TiFlash replica to the table

```
mysql> ALTER TABLE ontime SET TIFLASH REPLICA 1;
```

Then execute queries and inspect the `EXPAIN` and `EXPLAIN ANALYZE` output.

## Testing on Larger Scale
You can [download](https://github.com/Percona-Lab/ontime-airline-performance/blob/master/download.sh) more data and create a high concurrent insert to TiDB. Then run some complex queries that will read data from TiFlash. The goal is to check if these complex queries have an impact to the high concurrent insert. 