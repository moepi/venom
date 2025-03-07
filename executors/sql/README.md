# Venom - Executor SQL

Step to execute SQL queries into **MySQL** and **PostgreSQL** databases.

It use the package `sqlx` under the hood: https://github.com/jmoiron/sqlx to retreive rows as a list of map[string]interface{}

## Input

In your yaml file, you declare tour step like this

```yaml
  - driver mandatory [mysql/postgres]
  - dsn mandatory
  - commands optional
  - file optional
 ```

- `commands` is a list of SQL queries.
- `file` parameter is only used as a fallback if `commands` is not used.

Example usage (_mysql_):

```yaml

name: Title of TestSuite
testcases:

  - name: Query database
    steps:
      - type: sql
        driver: mysql
        dsn: user:password@(localhost:3306)/venom
        commands:
          - "SELECT * FROM employee;"
          - "SELECT * FROM person;"
        assertions:
          - result.queries.__len__ ShouldEqual 2
          - result.queries.queries0.rows.rows0.name ShouldEqual Jack
          - result.queries.queries1.rows.rows0.age ShouldEqual 21

```

```yaml

name: Title of TestSuite
testcases:

  - name: Query database
    steps:
      - type: sql
        database: mysql
        dsn: user:password@(localhost:3306)/venom
        file: ./test.sql
        assertions:
          - result.queries.__len__ ShouldEqual 1
```

*note: in the example above, the results of each command is stored in the results array

## SQL drivers

This executor uses the following SQL drivers:

- _MySQL_: https://github.com/go-sql-driver/mysql
- _PostgreSQL_: https://github.com/lib/pq
