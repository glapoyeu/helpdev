# MySQL: How to clone a database

Here is a way to create a duplicate of one database, with all its tables and their data, under a new name.

1. Make a dump of your source database:

```
mysqldump -uroot -p my_project -r my_project.sql

```

Or, if you only want to dump the database's table structure (schema) without any contents:

```
mysqldump -uroot -p my_project -r my_project.sql --no-data
```

2. Open up a MySQL shell:
```
mysql -uroot -p
```

3. From the MySQL shell, create a new database and populate it with the dumped data:

```
CREATE DATABASE my_project_copy;
USE my_project_copy;
SOURCE my_project.sql;
```

4. Create a user and give it permissions to the new database:
```
CREATE USER new_user IDENTIFIED BY 'some_password';
GRANT ALL ON my_project_copy.* TO 'new_user'@'localhost' IDENTIFIED BY 'some_password';
FLUSH PRIVILEGES;
```
