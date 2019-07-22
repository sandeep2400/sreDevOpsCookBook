# Mysql Replication

## Configure the master Server
* Edit the my.cnf file. In Ubuntu the my.cnf file is located at /etc/mysql/my.cnf
* Give it a unique server Id
```shell
server-id = 1
```
* Ensure there's a bimary log defined. check for the remaining 2 other parameters. 
```shell
log_bin                 = /var/log/mysql/mysql-bin.log
expire_logs_days        = 10
max_binlog_size         = 100M
```
* Restart the server once its done 
```shell
sudo service mysql restart
```


* Log into MYSQl on master mysql and create the replication user. - the user who will connect to the master server and cpy to it. 
```shell
GRANT replication slave on *.* to 'replication_user'@'10.0.0.10' Identified by 'your password'; 
FLUSH privileges;
```

## Configure Replica Server
* Log into the replica MYSQL server and edit the my.cnf file to ive it a unique id and setup some logs. Edit the **mysql/my.cnf**
* Give server a unique id
```shell
server-id = 2
```
* Ensure there is a relay log. 
```shell
relay-log  = /var/log/mysql/mysql-relay-bin.log
```
* Keep the binary log on. 
```shell
log_bin = /var/log/mysql/mysql-bin.log
```

* Restart the server once it is done 
```shell
sudo service mysql restart
```

## Once the user is created, we need to do a few things: 
* Lock the master database from accepting new queries that may change the database
* Get the current "position" of where MySQL is in the bin log (where it's left off writing to the binary log)
* Get the binary log file name
* What we're doing here is finding out exactly where MySQL is currently in writing to the binary log. We'll get this information, export the database in this state, and then release the lock.

## Lock Master Server & Get Position
* lock the master database and get it's status. Log into the MySQL server and run the following queries:
```shell
FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
```
* This should look something like this  - **SAVE this on a text file separately**
```shell
SHOW MASTER STATUS;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000001 |      120 |              |                  |
+------------------+----------+--------------+------------------+
1 row in set (0.00 sec)			
```

* This is the position from which the slave database will start replicating. Record these numbers, they will come in useful later.

* If you make any new changes in the same window, the database will automatically unlock. For this reason, you should open the new tab or window and continue with the next steps there.

## Backup master and restore to replica
* get an export of the database in its current state:	# Exporting one database specifically
```shell
mysqldump -u root -p newdatabase > newdatabase.sql
mysql -u root -p -e "UNLOCK TABLES;"
```

* Once we have the binary log information and a dump of the database, we can unlock it and let it continue on as normal.
```shell
mysql -u root -p -e "UNLOCK TABLES;"
```

## Start Replica at Binlog Position

* Create a new database (or any new databases) that is used on the replica
```shell
CREATE DATABASE newdatabase;
EXIT
```
* Import the master database dump into the replica database: We're assuming just one database is in both master and replicaSo I set the name of the database to import into
```shell
mysql -u root -p newdatabase < /path/to/newdatabase.sql			
```

* Finally, log into the replica database and let it know where to find the master database:
```shell
CHANGE MASTER TO MASTER_HOST='12.34.56.789',MASTER_USER='replication_user', MASTER_PASSWORD='some_password', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=  120;
```

* Once that's set, start the database "slave" operation, so it starts replicating from the master database
```shell
START SLAVE;
```

* Test Replica: To get the relica database's status, log into the replica database and run the following:
```shell
SHOW SLAVE STATUS;
```
