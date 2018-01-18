# Basics of IPTables

IPtables is a default firewall one finds in most places.

## View all tables 
```shell
sudo iptables -L -v
# L to list
# v verbose output
```

```shell
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination   
```
Default policy **ACCEPT**, no rules in each of the three chains. 

These are the chain of rules that dictate how incoming, output and forwarded traffic is handled.  
The **first rule** that meets the criteria is applied to the traffic. 

_Mostly we take action on INPUT chains, which monitors incoming packets of data_.  

## Basics
We'll setup a set of rules that will become the basic rules you can use on any server to start.  
First: Loopback/localhost data:  
Allow data between items on the localhost network (loopback interface).  

```shell
sudo iptables -A INPUT -i lo -j ACCEPT
# Append to INPUT chain
# interface loopback
# jump to ACCEPT target [packets get SENT somewhere]
```

Second, advanced but necessary:  (keep your current connections alive, or you will be knocked out of the server)
```shell
sudo iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
# Allow/keep current related/established rules, such as our SSH connection.

# Append to INPUT chain
# Use module conntrack
# Check for state related, established
# jump to accept
```
What's this affecting?  

```shell
netstat -a:
```
Find items on the top:
```shell
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 *:ssh                   *:*                     LISTEN     
tcp        0    324 172.30.0.86:ssh         cpe-173-174-200-3:61248 ESTABLISHED
tcp6       0      0 [::]:ssh                [::]:*                  LISTEN     
udp        0      0 *:bootpc                *:*                                
udp        0      0 *:40118                 *:*                                
udp6       0      0 [::]:46591              [::]:*
```

## The Usual Suspects: SSH & Web Traffic
Allow future SSH connections & HTTP (port 80) traffic: 

```shell
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```
* Append to INPUT chain
* Protocol TCP
* Destination port 22 and 80
* Jump to ACCEPT
* Drop everything else

```shell
sudo iptables -A INPUT -j DROP
```

* Append to INPUT chain
* Drop all the things

See [DROPing vs REJECTing](http://serverfault.com/questions/157375/reject-vs-drop-when-using-iptables) traffic. Now investigate:
```shell
sudo iptables -L -v
```

## HTTPS (Insert over Append)
We need to insert a rule instead of append one to allow other connections, such as https traffic: 
```shell
sudo iptables -I INPUT 5 -p tcp --dport 443 -j ACCEPT
```
* Insert to INPUT chain, 5th position
* Protocol TCP
* Destination port 443
* Jump to ACCEPT

## Delete
These are equivalent in our example:
```shell
# Delete rule in third position in list, starting with 1 (not zero)
sudo iptables -D INPUT 3
```

```shell
# Or delete the rule by matching the parameters
sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT
```

## Find Rules by Command
We can delete rules by find the command originally used to create them.  

To show all the current rules as valid Iptables command, run: 
```shell
sudo iptables -S
```

Copy one of them, perhaps this one: **-A INPUT -p tcp -m tcp --dport 22 -j ACCEPT**  
And then tweak it to delete **-D** rather than append or insert. We'll end up with a command like, which will delete this rule allowing port 22 traffic:

```shell
sudo iptables -D INPUT -p tcp -m tcp --dport 22 -j ACCEPT
```

Reference: https://serversforhackers.com/c/firewalls-basics-of-iptables