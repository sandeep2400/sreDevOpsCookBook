# the /etc/shadow file

The `/etc/shadow` file stores actual passwords in an encrypted format (more like the hash of the password) for a user’s account with additional properties related to user password.  
All fields are separated by a colon (:) symbol.  
It contains one entry per line for each user listed in /etc/passwd file. 

## A sample Shadow file
![Image of /etc/shadow](https://github.com/sandeep2400/sreDevOpsCookBook/blob/master/images/etc.shadow.png)

* Username : It is your login name.
* Password : It is your encrypted password. The password should be minimum 8-12 characters long including special characters, digits, lower case alphabetic and more. Usually the password format is set to **$id$salt$hashed**, The `$id` is the algorithm used On GNU/Linux as follows:
	* $1$ is MD5
	* $2a$ is Blowfish
	* $2y$ is Blowfish
	* $5$ is SHA-256
	* $6$ is SHA-512
* Last password change (lastchanged) : Days since Jan 1, 1970 that password was last changed
* Minimum : The minimum number of days required between password changes i.e. the number of days left before the user is allowed to change his/her password
* Maximum : The maximum number of days the password is valid (after that user is forced to change his/her password)
* Warn : The number of days before password is to expire that user is warned that his/her password must be changed
* Inactive : The number of days after password expires that account is disabled
* Expire : days since Jan 1, 1970 that account is disabled i.e. an absolute date specifying when the login may no longer be used.

## Password Aging and Lockout
The last 6 fields provides password aging and account lockout features.  
You need to use the `chage` command to setup password aging.

```shell
chage username
chage [options] username 
chage vivek
chage -l tom
```
The list of available options are:
```shell
	-d, --lastday LAST_DAY        set date of last password change to LAST_DAY
  	-E, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  	-h, --help                    display this help message and exit
  	-I, --inactive INACTIVE       set password inactive after expiration to INACTIVE
  	-l, --list                    show account aging information
  	-m, --mindays MIN_DAYS        set minimum number of days before password change to MIN_DAYS
  	-M, --maxdays MAX_DAYS        set maximim number of days before password change to MAX_DAYS
  	-R, --root CHROOT_DIR         directory to chroot into
  	-W, --warndays WARN_DAYS      set expiration warning days to WARN_DAYS
```

## Change passwords
Use the `passwd` command
```shell
$passwd
```

## Change passwords for other users
You must be root to change passwords
```shell
# passwd userNameHere
```

