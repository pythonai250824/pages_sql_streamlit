# Advanced PostgreSQL Stored Procedures and Queries

This page dives deeper into more advanced usages of stored procedures and functions in PostgreSQL, focusing on real-world examples involving multiple tables, joins, filters, and statistical aggregation

## 🎬 Example: Procedure to Prepare a Movie Database

```sql
drop procedure if exists prepare_db_movies;

create or replace procedure prepare_db_movies()
LANGUAGE plpgsql
AS $$
BEGIN
    CREATE TABLE IF NOT EXISTS countries (
        id   BIGSERIAL PRIMARY KEY,
        name TEXT NOT NULL UNIQUE
    );

    CREATE TABLE IF NOT EXISTS movies (
        id           BIGSERIAL PRIMARY KEY,
        title        TEXT,
        release_date TIMESTAMP NOT NULL,
        price        DOUBLE PRECISION DEFAULT 0 NOT NULL,
        country_id   BIGINT REFERENCES countries(id)
    );

    INSERT INTO countries(name) VALUES ('Israel');
    INSERT INTO countries(name) VALUES ('USA');
    INSERT INTO countries(name) VALUES ('JAPAN');
    INSERT INTO countries(name) VALUES ('CANADA');

    INSERT INTO movies (title, release_date, price, country_id)
    VALUES 
        ('batman returns', '2020-12-16 20:21:00', 45.5, 3),
        ('wonder woman', '2018-07-11 08:12:11', 125.5, 3),
        ('matrix resurrection', '2021-01-03 09:10:11', 38.7, 4);
END;
$$;

CALL prepare_db_movies();
```

**Explanation:**

* Creates two tables (`countries` and `movies`) if they do not exist
* Populates them with test data
* Demonstrates how to bundle multiple DDL and DML operations in one procedure

## 🎥 Example: Function to Get the Most Expensive Movie Price

```sql
drop function sp_get_expensive_movie();

CREATE OR REPLACE FUNCTION sp_get_expensive_movie() returns double precision
LANGUAGE plpgsql AS
$$
DECLARE
  max_price double precision;
BEGIN
    select max(price) INTO max_price
    from movies;
    return max_price;
END;
$$;

select sp_get_expensive_movie();
```

**Explanation:**

* Returns the maximum price from the `movies` table
* Uses a local variable and `INTO` keyword

## 🧾 Function with OUT Parameter

```sql
drop function sp_get_expensive_movie2();

CREATE OR REPLACE FUNCTION sp_get_expensive_movie2(out max_price double precision)
LANGUAGE plpgsql AS
$$
BEGIN
    select max(price) INTO max_price
    from movies;
END;
$$;

select sp_get_expensive_movie2();
```

**Explanation:**

* Returns the same result as the previous example
* Uses `OUT` parameter instead of `RETURN`

## 📊 Function to Return Movie Statistics

```sql
drop function sp_movies_stat();

CREATE or replace function sp_movies_stat(
    OUT min_price double precision,
    OUT max_price double precision,
    OUT avg_price double precision,
    OUT count_movies INTEGER)
language plpgsql AS
$$
BEGIN
    select min(price), max(price), avg(price)::numeric(5,2), count(*)
    into min_price, max_price, avg_price, count_movies
    from movies;
END;
$$;

select * from sp_movies_stat();
```

**Explanation:**

* Returns four statistics: min, max, average (rounded), and total count of movies
* Demonstrates use of multiple `OUT` parameters

## 🎞️ Function to Return All Movies

```sql
drop function sp_get_all_movies();

CREATE or replace function sp_get_all_movies()
returns TABLE(id bigint, title TEXT, release_date TIMESTAMP, price DOUBLE PRECISION, country_id bigint)
language plpgsql AS
$$
BEGIN
    return QUERY
    select * from movies;
END;
$$;

select * from sp_get_all_movies();
```

**Explanation:**

* Uses `RETURNS TABLE` syntax to return all rows from the `movies` table

## 🌍 Function with JOIN (Movies + Country Name)

```sql
drop function sp_get_all_movies_country_name();

CREATE or replace function sp_get_all_movies_country_name()
returns TABLE(id bigint, title TEXT, release_date TIMESTAMP, price DOUBLE PRECISION, country_name TEXT)
language plpgsql AS
$$
BEGIN
    return QUERY
    select m.id, m.title, m.release_date, m.price, c.name as country_name
    from movies m join countries c on m.country_id = c.id;
END;
$$;

select * from sp_get_all_movies_country_name();
```

**Explanation:**

* Joins movies with countries to include country names in the result

## 🎯 Function with Price Range Filtering

```sql
drop function sp_get_all_movies_country_name_range();

CREATE or replace function sp_get_all_movies_country_name_range(_min double precision, _max double precision)
returns TABLE(id bigint, title TEXT, release_date TIMESTAMP, price DOUBLE PRECISION, country_name TEXT)
language plpgsql AS
$$
BEGIN
    return QUERY
    select m.id, m.title, m.release_date, m.price, c.name as country_name
    from movies m join countries c on m.country_id = c.id
    where m.price between _min and _max;
END;
$$;

select * from sp_get_all_movies_country_name_range(40, 130);
```

**Explanation:**

* Adds filtering on price using parameters `_min` and `_max`
* Useful for reporting and dashboards

This advanced set of examples shows the full power of PL/pgSQL to create smart, reusable database logic for data manipulation, reporting, and business logic
