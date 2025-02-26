# Database
## Overview

Database connection is configured in the `db` section of config file.

Currently supported databases:

- SQLite
- MySQL
- PostgreSQL

## SQLite
To use SQLite specify `sqlite` in `db.engine` configuration parameter.

Example config:
```
db:
  engine: sqlite
  filename: gorm.db
```

### engine
`sqlite` to use SQLite database

### filename
You can also override database file path if needed.


## MySQL
Example config:
```
db:
  engine: mysql
  mysql:
    host: 127.0.0.1
    port: 3306
    username: admin
    password: changeme
    database: passage

```

### engine
`mysql` to use MySQL database

### mysql.host
Hostname of MySQL server instance

### mysql.port
Port of MySQL server

### mysql.username
Username to use when connecting to MySQL server

### mysql.password
Password to use when connecting to MySQL server

### mysql.database
Database name


## PostgreSQL
Example config:
```
db:
  engine: psql
  mysql:
    host: 127.0.0.1
    port: 3306
    username: admin
    password: changeme
    database: passage
    schema: public

```

### engine
`psql` to use PostgreSQL database

### psql.host
Hostname of PostgreSQL server instance

### psql.port
Port of PostgreSQL server

### psql.username
Username to use when connecting to PostgreSQL server

### psql.password
Password to use when connecting to PostgreSQL server

### psql.database
Database name

### psql.schema
Schema name
