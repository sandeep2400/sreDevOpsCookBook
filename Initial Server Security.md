# Initial Server Security

Usually servers are assigned with a public IP and hosting providers set up with password-based access to the root user, or a SUDO user who does not need a password to run commands (like vagrant).

* Root users can do anything on the system, it is important to lock down the ability for remote connections to log in as root. 
* We will need another user to be able to run sysadmin ops.  User should need a password to run privileged commands. 

## Steps:

1 Create a New user 
2 Assign user sudo rule. 
3 Stop root user from remotely logging in. 
4 Prevent users from logging in w/  a Pswd. 
5 Only allow SSH logins.
		
### Login to the Server
```shell
$ssh root@your-server-ip
```
or 
```shell
$ssh -I /path/to/identity.pem root@your-server-ip (if Aws requires identity file). 
```

	


