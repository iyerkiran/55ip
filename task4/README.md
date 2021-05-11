# Task4

## Problem Statement:
Description: There is remote database server(RDBMS-MySQL,Postgresql) running on ip address 10.0.0.10.
write a shell script to
1. Create the Database
2. Create a table & insert data
3. Alter the same table and drop the table
4. Drop the database

## Proposed Solution:
Assumptions:
1. DB user crednetials are passed in the script itself.
2. Connectivity to the remote db host is valid.
3. No SSL connection assumed with DB


```sh
#!/bin/bash
db_name=census
db_pass=1e69dc33541218abf851f0e671066994
db_user=administrator
table_name=population
host_ip=10.0.0.10

# Create a User for DB
mysql -u root -h $host_ip -p -e "CREATE USER '$db_user'@'localhost' IDENTIFIED BY '$db_pass'";
mysql -u root -h $host_ip -p -e "GRANT ALL PRIVILEGES ON $db_name.* TO '$db_user'@'localhost' IDENTIFIED BY '$db_pass'";
mysql -u root -h $host_ip -p -e "FLUSH PRIVILEGES";

# Create the database:
mysql -u $db_user -h $host_ip -p$db_pass -e "CREATE DATABASE IF NOT EXISTS $db_name";

# Create table and insert data:
mysql -u $db_user -h $host_ip -p$db_pass --database=$db_name -e "CREATE TABLE IF NOT EXISTS $table_name ( age smallint, name varchar(20) not null, city varchar(30))";
mysql -u $db_user -h $host_ip -p$db_pass --database=$db_name -e "INSERT INTO $table_name ( age, name, city ) VALUES ( 30, 'John Doe', 'Kolkata' ), ( 46, 'Riki Marsh', 'Mumbai' )";

# Alter the same table
mysql -u $db_user -h $host_ip -p$db_pass --database=$db_name -e "ALTER TABLE $table_name ADD email varchar(255)";

# Drop table
mysql -u $db_user -h $host_ip -p$db_pass --database=$db_name -e "DROP TABLE $table_name";

# Drop DB
mysql -u $db_user -h $host_ip -p$db_pass -e "DROP DATABASE $db_name";
```
