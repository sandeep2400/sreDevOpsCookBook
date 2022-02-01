# Resetting MYSQL (ver 8.0) Root Password

To reset your root password, follow these steps:

## Stop the MySQL service

```shell
sudo /etc/init.d/mysql stop

sudo /etc/init.d/mysqld stop # Fedora, Centos, RedHat
```

## Start MySQL without a password

```shell
sudo mysqld_safe --skip-grant-tables &
```

## Connect to MySQL

```shell
mysql -u root
```

## Set a new MySQL root password
```shell
use mysql;
```
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_p@ssword';
```

```shell
flush privileges;
quit
```

## Stop and start the MySQL service
```shell
sudo /etc/init.d/mysql stop
```

```shell
sudo /etc/init.d/mysql start
```
_if this fails, run `ps -A` and `kill` all `mysql` and `mysqld` jobs with `kill -9 pid`_

After that rerun the start command 

```shell
sudo /etc/init.d/mysql start
```

