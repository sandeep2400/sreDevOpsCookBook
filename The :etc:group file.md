# The :etc:group file

`/etc/group` is a text file which defines the groups to which users belong under Linux and UNIX operating system.  
Under Unix / Linux multiple users can be categorized into groups.  
Unix file system permissions are organized into three classes, `user`, `group`, and `others`. 
The use of groups allows additional abilities to be delegated in an organized fashion, such as access to disks, printers, and other peripherals.  
This method, amongst others, also enables the Superuser to delegate some administrative tasks to normal users.  

## Anatomy of the groups file
There is one entry per line, and each line has the following format (all fields are separated by a colon (:)
![Image of /etc/group](https://github.com/sandeep2400/sreDevOpsCookBook/blob/master/images/etc.group.jpg)

* group_name: It is the name of group. If you run ls -l command, you will see this name printed in the group field.
* Password: Generally password is not used, hence it is empty/blank. It can store encrypted password. This is useful to implement privileged groups.
* Group ID (GID): Each user must be assigned a group ID. You can see this number in your /etc/passwd file.
* Group List: It is a list of user names of users who are members of the group. The user names, must be separated by commas.

### List all users in a group
```shell 
$ groups {username}
$ groups
$ groups vivek
```

Reference: https://www.cyberciti.biz/faq/understanding-etcgroup-file/