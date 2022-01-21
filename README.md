# DB TESTS

## How to create test data

```shell
host1812@vm-aftest-mysqlclient:~$ sysbench \
> --db-driver=mysql \
> --table-size=10000000 \
> --tables=1 \
> --threads=1 \
> --mysql-host=db-aftest-failover-db1.mysql.database.azure.com \
> --mysql-port=3306 \
> --mysql-user=other \
> --mysql-password=other \
> --mysql-db=sysbench \
> /usr/share/sysbench/oltp_read_only.lua \
> prepare
sysbench 1.0.18 (using system LuaJIT 2.1.0-beta3)

Creating table 'sbtest1'...
Inserting 10000000 records into 'sbtest1'
Creating a secondary index on 'sbtest1'...
```

## How to crash

```shell
for i in {1..48}; do (mysql -uother -pother -h db-aftest-failover-db1.mysql.database.azure.com -e 'SELECT * FROM sysbench.sbtest1 ORDER BY c DESC' >/dev/null &); done
```

## read-write

```shell
sysbench \
> --db-driver=mysql \
> --table-size=10000000 \
> --tables=1 \
> --threads=1 \
> --mysql-host=db-aftest-failover-db1.mysql.database.azure.com \
> --mysql-port=3306 \
> --mysql-user=other \
> --mysql-password=other \
> --mysql-db=sysbench \
> /usr/share/sysbench/oltp_read_write.lua \
> prepare
```