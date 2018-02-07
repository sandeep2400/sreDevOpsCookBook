[mysql]
# CLIENT #
port                           = 3306
socket                         = /mnt/database/mysql.sock


[mysqld]

port                           = 3306
socket                         = /mnt/database/mysql.sock

# GENERAL #
user                           = mysql
default-storage-engine         = InnoDB
socket                         = /mnt/database/mysql.sock
pid-file                       = /mnt/database/mysql.pid
wait_timeout		       = 30
relay-log                      = /mnt/logs/relay-logs/relay-bin.log
default-time-zone              = US/Eastern
sql_mode                       = NO_ENGINE_SUBSTITUTION

# BINARY LOGGING #
binlog_format                  = row
log_bin			       = /mnt/logs/bin-log/mysql-bin
expire-logs-days               = 14
sync-binlog                    = 1
server-id                      = 10
log-slave-updates              = 0

# MyISAM #
key-buffer-size                = 128M
#myisam-recover                = FORCE,BACKUP
read_rnd_buffer_size	       = 2M

# SAFETY #
max-allowed-packet             = 256M
max-connect-errors             = 1000000
skip-name-resolve	       = 1
sysdate-is-now                 = 1
innodb                         = FORCE
#innodb-strict-mode             = 1

# DATA STORAGE #
datadir                        = /mnt/database

# CACHES AND LIMITS #
tmp-table-size                 = 32M
max-heap-table-size            = 32M
query-cache-type               = 0
query-cache-size               = 0
max-connections                = 1500
thread-cache-size              = 50
open_files_limit               = 65535
table-definition-cache         = 4096
table-open-cache               = 4096
#query_cache_size	       = 64M
#sort_buffer_size	       = 2M
#read_buffer_size	       = 2M
#max_heap_table_size	       = 128M
#tmp_table_size		       = 128M
wait_timeout                   = 28800

# INNODB #
innodb-flush-method            = O_DIRECT
innodb-log-files-in-group      = 2
innodb-log-file-size           = 1G
innodb-flush-log-at-trx-commit = 1
innodb-file-per-table          = 1
innodb-buffer-pool-size        = 45G

# LOGGING #
log-error                      = /mnt/logs/mysql/mysql-error.log
slow-query-log-file            = /mnt/logs/mysql/slow.log

