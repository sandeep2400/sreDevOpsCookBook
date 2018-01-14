### Permissions and User Management

## Permissions
For every directory and file, there are permissions that can be set informing what users can perform operations on them, as well as what operations those users can take.  

Users can perform r**ead (r)**, **write (w)** and **execute (x)** operations on files/directories. Here's how the three permission types breaks down when applied to directories and files:
* Directories
	* read - ability to read contents of a directory
	* write - ability to rename or create a new file/directory within a directory (or delete a directory)
	* execute - ability to cd into a directory
* Files
	* read - ability to read a file
	* write - ability to edit/write to a file (or delete a file)
	* execute - ability to execute a file (such as a bash command)

The other half of this is users and groups. For any file and directory, we can define how users (u), groups (g) and others (o) can interact with a directory or file. Here's how that breaks down:

* User - The permission for owners of a file or directory
* Group - The permissions for users belonging to a group. A user can be part of one or more groups. Groups permissions are the primary means for how multiple users can read, write or execute the same sets of files
* Other - The permissions for users who aren't the user or part of a group assigned to a file or directory

## Checking Permissions
To illustrate this, let's check the permissions of a directory, for example /var/www:
```shell
$ ls -la /var/www
drwxr-xr-x  2 root root 4096 May  3 19:52 .          # Curent Directory
drwxr-xr-x 12 root root 4096 May  3 19:46 ..         # Containing Directory
-rw-r-xr--  1 root root   13 May  3 19:52 index.html # File in this Directory
```
How do these columns of information break down?

* drwxr-xr-x - User/Group/Other Permissions. The preceding "d" denotes this as a directory. Lacking a "d" means it's a file.
* 2 - This is the number of "hard links" to the file or directory
* root root - The User and Group assigned to the file or directory
* 4096 - The size of the file/directory in bytes
* May 3 19:52 - last modified (or created) data/time
* . - The file name. A period (.) is the current directory. Two periods (..) is the directory one level up. Otherwise this will show a file or directory name.

Let's go over the permission attributes - that first column of information:  
For any permission attribute set, the first slot denotes if it's a **directory (d), link (l) (as in symbolic link) or file (-)**. 

Then each next three characters denotes the read, write and execute permissions for users groups and others, respectively.

Let's take the permissions drwxr-xr-x.

* d - denotes it's a directory
* rwx - The user has read, write and execution permissions
* r-x - The group can read and execute, but not write
* r-x - The same for others. Since this is a directory, this means users can read the directory but not modify it or its containing files

Next, let's analyze -rw-r-xr--:

* - - denotes it's a file
* rw- - denotes users can read, write but not execute the file
* r-x - group members can read the file or execute it
* r-- - others can only read the file

##Changing Permissions
We can change a file or directory permissions with the chmod command.  
Here's some chmod information and a breakdown:  

```shell
chmod [-R] guo[+-]rwx /var/www
```

Flags:
* -R - Change permissions recursively (if its a directory)

User types:
* u - perform operation on the user permissions
* g - perform operation on the group permissions
* o - perform operation on the other permissions

Operations:
* + - add permission
* - - remove permission

Permission types to set
* r - add or remove read permissions
* w - add or remove write permissions
* x - add or remove execute permissions

So, for example, let's create the /var/www directory as user root and set its permissions.

```shell
# Create directory as root
sudo mkdir /var/www

# Change to owner/group www-data:
sudo chown www-data:www-data /var/www

# Set permissions so user and group has permissions, but not other:
sudo chmod ug+rwx /var/www # User and Group have all permissions (+rwx)
sudo chmod o-rwx /var/www  # Other has no permissions (-rwx)
sudo chmod o+rx /var/www   # Other can read and `cd` into the directory (+rx)
```

This is useful if you have a user for deployment on your server. If the deployment user is part of the group `www-data`, that user will have permissions to add/update files within the `/var/www directory`.  
Note that files created by a user are set to belong to that users username and main group _(users belong to a main and secondary groups)_. This means that after creating/deploying files, we'll likely need to set their permissions properly. For example, after a deployment to `/var/www`, the deployment user should set the group and group permissions of all new files within `/var/www/` so the `www-data` group and read/write to them, and any "others" can only read them.


Reference: https://serversforhackers.com/c/permissions-and-user-management