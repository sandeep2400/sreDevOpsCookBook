# The /etc/passwd File
The `/etc/passwd` file stores essential information, which is required during login i.e. user account information.  
It is a text file, which contains a list of the system’s accounts, giving for each account some useful information like user ID, group ID, home directory, shell, etc.  
It should have general **read permission** as many utilities like `ls` use it to map user IDs to user names, but write access only for the `superuser/root` account.

## Understanding fields in /etc/passwd
The file contains one entry per line for each user (or user account) of the system.  
All fields are separated by a **colon (:)** symbol.
The seven fields as follows. Generally, passwd file entry looks as follows (click to enlarge image):  
![Image of /etc/passwd](https://github.com/sandeep2400/sreDevOpsCookBook/blob/master/images/etc_passwd.webp)

* Username: It is used when user logs in. It should be between 1 and 32 characters in length.
* Password: An x character indicates that encrypted password is stored in the `/etc/shadow` file. Please note that you need to use the passwd command to compute the hash of a password typed at the CLI or to store/update the hash of the password in `/etc/shadow` file.  
* User ID (UID): Each user must be assigned a user ID (UID). _UID 0 (zero) is reserved for root and UIDs 1-99 are reserved for other predefined accounts_. Further UID 100-999 are reserved by system for administrative and system accounts/groups.
* Group ID (GID): The primary group ID (stored in `/etc/group` file)
* User ID Info: The comment field. It allow you to add extra information about the users such as user’s full name, phone number etc. This field use by finger command.
* Home directory: The absolute path to the directory the user will be in when they log in. If this directory does not exists then users directory becomes `/`
* Command/shell: The absolute path of a command or shell (/bin/bash). Typically, this is a shell. Please note that it does not have to be a shell.


