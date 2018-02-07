# MYSQL Server Checklist

* Create a Linux Ubuntu VM on Azure. 
* Uninstalled MySQL 5.7 and replaced it with MySQL 5.6
* Opened up ports 22 and 3306
* Set up a mysql.cnf file. 
* imported the database from a mysqldump. 
	* replaced ISAM with InnoDB. 
	* removed row_format = fixed from the mysqldump
* restart mysql
* test site. 
