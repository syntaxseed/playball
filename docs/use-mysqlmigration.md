# Using The MySQL Migration Role

The `deploy-example.yml` playbook contains the main features of PHP Playball. This file configures whether you are running PHP deployments and/or running MySQL Migrations.

> Note that MySQL Migrations cannot currently be rolled back.

Enable MySQL Migrations by setting the `RUN_MYSQL_MIGRATIONS` variable to true.
```
RUN_MYSQL_MIGRATIONS: true
```

## Configuration

### Configure Mysql Details

Set the name of the database to run migrations on, and the path to the `.my.cnf` file which must contain the user and password. It should look like this:
```
[client]
    user=dbuser
    password=dbpassword
    host=localhost
```

### MYSQL_MIGRATIONS_PATH

The path to find the timestamp named SQL migration files. These will be run against the database only once.

## Creating Migrations

When working on your project, changes to the database should be defined in dated SQL files stored in our migrations directory. File names look like: 20190625210800_add_table.sql. They must begin with a YYYYMMDDHHMMSS timestamp followed by an underscore.

You should wrap the SQL in these files in a transaction, to prevent partial execution due to errors. You should also write your SQL to check if a table already exists before creating it and other idempotent strategies to minimize damage if a migration is executed twice (ie due to errors or a deleted dat file).

The first SQL file with the earliest timestamp should set up the initial state of the database. This will run on your first migration.

## How Migrations Are Run

Ansible stores the last timestamp of the last migration run in the file `{ansibleprivatepath}/mysql_migrations.dat`. Note this is different per site/deployment on the server. The first time you run a deployment, this file won't exist, Ansible will create it for you.

Ansible looks into your migrations path (`MYSQL_MIGRATIONS_PATH`) and reads the list of all SQL files with filenames timestamped AFTER the last timestamp retrieved from the `mysql_migrations.dat`.

These files are run against the database in order.

The timestamp of the last run file is then saved in `mysql_migrations.dat`. This is how PHP Playball knows not to run the same migrations more than once.

TIP: If multiple developers are working on the project, you should co-ordinate the creation of SQL Migrations to ensure the timestamps keep them in the proper order.
