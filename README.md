# InterSystems IRIS DB-API Community Driver

## NOTE: This application is not supported by InterSystems Corporation. Please be notified that you use it at your own risk. 

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/intersystems-community/intersystems-irispython)
](https://github.com/intersystems-community/intersystems-irispython/releases/latest)

## Install

```shell
pip install https://github.com/intersystems-community/intersystems-irispython/releases/download/3.5.0/intersystems_iris-3.5.0-py3-none-any.whl
```

## Examples

Connect to IRIS over network

```python
import intersystems_iris.dbapi._DBAPI as dbapi

config = {
    "hostname": "localhost",
    "port": 1972,
    "namespace": "USER",
    "username": "_SYSTEM",
    "password": "SYS",
}

with dbapi.connect(**config) as conn:
    with conn.cursor() as cursor:
        cursor.execute("select ? as one, 2 as two", 1)   # second arg is parameter value
        for row in cursor:
            one, two = row
            print(f"one: {one}")
            print(f"two: {two}")
```

Connect to IRIS using embedded python mode. `%Service_CallIn` have to be enabled in IRIS

```python
import intersystems_iris.dbapi._DBAPI as dbapi

config = {
    "embedded": True,
    "namespace": "USER",
}

with dbapi.connect(**config) as conn:
    with conn.cursor() as cursor:
        sql = "select ? as one, ? as two union all select ?, ?"
        params = [1, 2, 3, 4]
        cursor.execute(sql, params)
        for row in cursor:
            one, two = row
            print(f"one: {one}")
            print(f"two: {two}")
```

## Used by

This project already used by several projects

* [SQLAlchemy-IRIS](https://github.com/caretdev/sqlalchemy-iris) dialect for [SQLAlchemy](https://www.sqlalchemy.org/) framework. Which Can be used in [Pandas](https://pandas.pydata.org/), [Flask](https://flask.palletsprojects.com/) and many other libraries
* [Django-IRIS](https://github.com/caretdev/django-iris) driver for [Django](https://www.djangoproject.com/) full-stack framework
* [dbt-iris](https://github.com/caretdev/dbt-iris) plugin to [DBT](https://www.getdbt.com/) to make ELT possible with IRIS
