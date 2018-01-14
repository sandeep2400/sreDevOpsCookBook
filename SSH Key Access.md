# SSH Key Access

### Generate an SSH Key Pair
```shell
$ cd ~/.ssh
$ ssh-keygen -t rsa -b 4096 -C your@email.com -f id_myserveridentity
```
This is what the command stands for:
* -t rsa: Create an RSA Key type
* -b 4096: Use 4096 bits. 2048 is "usually sufficient", but I go higher
* -C your@gmail.com: THis is a comment, a user's identity goes here.
* -f id_myserveridentity: The name of the files created (id_myserveridentity and id_myserveridentity.pub in this case)

_A password is highly recommended for further security_

Now we've created a private key file (id_myserveridentity) and a public key file (id_myserveridentity.pub). We need to put the public key on the server, so that the server knows its a key authorized to be used to log in. Copy the contents of the public key file (on Mac: `cat ~/.ssh/id_myserveridentity.pub | pbcopy` will add it to your clipboard).

```shell
# In your server
# Use nano or vim
$ mkdir ~/.ssh  # ~/.ssh dir may not exist yet
$ nano ~/.ssh/authorized_keys
> Paste in your public key and save/exit
```
Once the authorized_keys file is saved, you should be able to login using your key. You shouldn't need to do anything more, it should attempt your keys first and, finding one, log in using it. You'll need to enter in your password created while generating your SSH key, if you elected to use a password

### Disable Password Logins
The last thing we'll do is to tell our server to only allow remote access via SSH keys (by turning off the ability to log in using a password). In `/etc/ssh/sshd_config`
```shell
PasswordAuthentication no
```
Save that file, and once again reload the SSH daemon:

```shell
# Debian/Ubuntu
$ sudo service ssh restart

# RedHat/CentOS
sudo service sshd restart
```

reference https://serversforhackers.com/c/initial-security-setup
