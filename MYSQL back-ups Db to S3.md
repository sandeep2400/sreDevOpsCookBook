# MYSQL back up Db to S3

## Command to backup a database using MYSQLDUMP
_Note: This is not a hot backup, the database tables are locked when a backup is completing._

```shell
mysqldump -u username -p {db_name} > some_database.sql # - basic command. Prompted for password. 

# gzip
mysqldump -u username -p {db_name} | gzip > some_database.sql.gz

#the --no-tablespaces is a workaround to giving the mysql user Process Privileges.
# Note here we are adding a date suffix to the bkup file. 
mysqldump --no-tablespaces -u username {db_name} |gzip > ~/bkups/database/{bkup_name}_`date +%Y_%m_%d_%H_%M`.sql.gz

```
## Storing Mysql Credentials
To avoid storing credentials in the command line you can save them to a `~/.my.cnf` file in your home directory like so
```shell
# ~/.my.cnf file
[mysqldump]
user=username
password=password
```

## Deleting old backups
You can delete old backup files that were created 20 days prior. 
```shell
	find ~/bkups/database/ -name "bkup_*.sql" -mtime +20 -exec /bin/rm {} \;
```

## Sync to S3
To send backups to S3 follow these steps:
* Create an S3 Bucket and an IAM user with only permissions to write bkups to the bucket (`PutObject` and `PutObjectAcl` ).

* The download and install AWSCLI to the server
``` shell
# Install Pip
sudo apt-get install -y python-pip

# Install AWS CLI tol globally
sudo pip install awscli

# Check to ensure it exists
which aws

# Configure AWS - adding the id, key, default zone and default output format
aws configure
> YOUR KEY HERE
> YOUR SECRET KEY HERE
> us-east-1
> json
```
* Then create a backup script that does everything you want like so

```bash
#!/bin/bash

FILE_NAME="example_db-`date +%Y%m%d%H%M`.sql.gz"
SAVE_DIR="/home/ubuntu/backup"
S3BUCKET="sfh-backup-25kdjds"

# Get MYSQL_USER and MYSQL_PASSWORD
source /home/ubuntu/backup/.env

mysqldump -u ${MYSQL_USER} -p${MYSQL_PASSWORD} example_db | gzip > ${SAVE_DIR}/${FILE_NAME}

if [ -e ${SAVE_DIR}/${FILE_NAME} ]; then

    # Upload to AWS
    aws s3 cp ${SAVE_DIR}/${FILE_NAME} s3://${S3BUCKET}/${FILE_NAME}

    # Test result of last command run
    if [ "$?" -ne "0" ]; then
        echo "Upload to AWS failed"
        exit 1
    fi

    # Exit with no error
    exit 0
fi

find /path/to/backup -name "*.sql*" -mtime +60 -exec /bin/rm {} \;

# Exit with error if we reach this point
echo "Backup file not created"
exit 1

```
## Finally run this on an hourly cron
```shell
# for current user
crontab -e

# for specific user:
crontab -u username -e

0 0 * * * /usr/bin/env bash /home/ubuntu/backup/backup.sh &>> /home/ubuntu/backup/backup.log
```

## References
* [Servers for Hackers](https://serversforhackers.com/c/backup-and-restore-mysql-with-mysqldump) - mysqldump
* [Servers for Hackers](https://serversforhackers.com/c/backup-to-s3) - sync to S3. 
* [Stack Overflow](https://stackoverflow.com/questions/9293042/how-to-perform-a-mysqldump-without-a-password-prompt) - how to mysqldump without a username and password
