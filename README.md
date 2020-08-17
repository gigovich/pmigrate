# pMigrate
Simple bash migration tool for PostgreSQL.

This tool helps to write simple migration files for PostgreSQL database.
It tracks migration numbers in the database through sequence number and uses
simple SQL files as migrations. All you need to start use this tool:
 * copy 'pmigrate' script to the folder in your project (where you plan to store migrations)
 * update database user, name and host in this script
 * create/update ~/.pgpass file with password for your database
 * create migrations SQL files with number prefixes like "0001\_create\_user\_table.sql" etc.
 * enjoy! Run `./pmigrate`

## How it works
**pMigrate** for the first run it creates `migration_number` sequence in the PostgreSQL database.
It lists all files with \*.sql extension and number prefix. Then it exequtes only files with
prefix number greater then database `migration_number` is, after each success migration
sequence number in the database increased, and database schema dump updated (it placed in the `0000_schema_dump.sql` file).

## How to create migraton file
Migration file mandatory requirement is number prefix in the name, like:
 * 0001\_create\_user\_table.sql
 * 0002\_seed\_users.sql
 * 0003\_create\_email\_indes.sql
 * 0004\_create\_orders\_table.sql
Content of the migration file should be any valid SQL queries. Don't wrap
it with `BEGIN;...COMMIT'` expresions, `pMigrate` does it for you. So regular migration file
can be like that:
```
CREATE TABLE "user" (
    id serial PRIMARY KEY,
    email text,
    password text,
    roles text[]
);
```

## Database credentials
This script is pretty simple, so database **username**, **hostname** and **database name**
you can set directly inside script. But with password, if you don't want type it each time
when script calls `psql` to invoke next step, best option will be to create/update `~/.pgpass` file.
In this file you can set password for any database ([more details](https://www.postgresql.org/docs/9.3/libpq-pgpass.html)).
