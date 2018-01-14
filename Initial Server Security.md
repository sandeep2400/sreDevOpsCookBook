# Initial Server Security

Usually servers are assigned with a public IP and hosting providers set up with password-based access to the root user, or a SUDO user who does not need a password to run commands (like vagrant).

* Root users can do anything on the system, it is important to lock down the ability for remote connections to log in as root. 
* We will need another user to be able to run sysadmin ops.  User should need a password to run privileged commands. 

## Steps:

1. Create a New user 
2. Assign user sudo rule. 
3. Stop root user from remotely logging in. 
4. Prevent users from logging in w/  a Pswd. 
5. Only allow SSH logins.
		
### Login to the Server
```shell
$ssh root@your-server-ip
```
or 
```shell
$ssh -I /path/to/identity.pem root@your-server-ip (if Aws requires identity file). 
```
### Create a new User (this command may ask for a password, choose a lengthy, complicated one). 
```shell
$adduser {{someusername}}
```
### Make this user a Sudo user. 
```shell
$usermod -a -G sudo {{someusername}}
	-a : Append the group to the user's group. 
	-G assign the group "sudo" as a secondary groups. 
```
### Stop root user from remotely logging in. 
* Edit the /etc/ssh/sshd_config file. 

```shell
Set PermitRootLogin no
```
* You can also change  the default SSH port 22 to something else on the Port 1234 line
* Or you can also explicityly define a list of users that can login. 
	
### Stop root user from remotely logging in. 
To do this we need to edit the `/etc/ssh/sshd_config` file. 
```shell
PermitRootLogin no
```
You may also want to change the default SSH port (22) used. This is because a lot of automated systems scan port 22 to see if its an open for attack. You're allowed to use ports between 1025 and 65536. To do so, you can simply change the Port option
```shell
Port 1234 # But don't use "1234"
```
If you want to get even more secure, you can explicitly define a list of users who are allowed to login. That's with the AllowUsers option:
```shell
AllowUsers someusername someotherusername
```

### Save the file and reload the SSH daemon.

```shell
# On Debian/Ubuntu
$ service ssh restart

# RedHat/CentOS
$ service sshd restart
```
Before you close your session as user root, I suggest you now open a new terminal window (session) and attempt to log in with your new user
```shell
# If you left the default port:
$ ssh someusername@your-server-ip

# If you changed the SSH port number:
$ ssh -p 1234 someusername@your-server-ip
```



