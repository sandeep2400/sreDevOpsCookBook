# Logging Basics
Logs are important to application developers. They give us detailed error information about our applications and the systems supporting them. They also can give us clues about what may go wrong in the future.  

## Where To Find Them
### Logs are almost always found in /var/log.  
Usually they are owned by a privileged user and group, such as `root` or `adm`. 
Even if you don't have access to a privileged user (a user who can use sudo), you can often - but not always - read these files.  
Within the `/var/log` directory, you'll find system and software logs. For example, in Debian/Ubuntu systems, you'll find your Apache access and errors logs in `/var/log/apache2`. In other OS distributions, this may be `/var/log/httpd`. Nginx logs are usually found in `/var/log/nginx`.

Not all log files are in a subdirectory. For example, if you're using PHP-FPM, you might find logs in `/var/log/php5-fpm.log`. MySQL is often similarly found at `/var/log/mysql.log` and `/var/log/mysql.err`.

Lastly, logs are often split between **access (or events)** and **error logs**. Apache, Nginx and MySQL all have separate error and access/use logs.

### Application Logs
Some web applications create their own logs. You should consult your framework's documentation to find out where they might be. The Laravel PHP framework, for example, will create logs in the app/storage/logs directory. To avoid getting a WSOD upon an error, you should make sure that the log directory can be written to.

## Log information on other popular frameworks:
* laravel (PHP): Uses Monolog under the hood
* sailsJS (Node): Uses Winston under the hood
* django (Python)
* flask (Python)
* RoR (Ruby)

## Watching Logs With Tail
Since logs may give you more error information than the browser when developing a web application, you may find it appropriate to monitor a log file. This is especially true in production environments where error reporting is turned off - they will only be displayed within log files.  
To monitor a log file, we can "tail" it. This will show the last n lines of the file as they are written to it.  

If I wanted to watch the last 50 lines of my apache error log for errors, I would run:
```shell
tail -n 50 -f /var/log/apache2/error.log
```
## Virtual Hosts and Logs
If you read the previous newsletter on Apache virtual hosts, you may have noticed that I setup a unique access and error log for each virtual host. This lets you track the logs of just one website, rather than having multiple website events combined into one log.For a site example.com, the log files might be /var/log/apache2/example.com-access.log and /var/log/apache2/exampe.com-error.log.

## Analyzing Log Files
Here are some simple examples of examining some Apache (or Nginx!) log files.  
Some of the command line tools available for things like are `grep`, `zgrep`, `awk`, `cut`, `sed`, `sort`, `uniq`, `head` and others. 

## More Advanced Analysis and Tooling
As you might imagine, there are LOTS of tools available to analyze and store logs. This becomes especially important if you have a multiple-server environment, each of which may be generating it's own collection of logs.  
In situations like this, a common strategy is to combine log files to a central storage. This usually comes with a bonus of being both searchable and more easily analyzed.

## Here's a list of some available logging tools:
### Self Install
* Logstash - An easy to use Open Source log and event manager. Numerous people pointed out that I missed this at first. It's one of the easiest (and backed by Elasticsearch) open-source loggers!
* Webalizer - Good for single servers, and often found in hosting accounts, this is good for basic analyzing of web traffic from Apache logs
* Fluentd - Open source tool to collect events and logs (in JSON)
* Graylog2 - This both collects logs AND analyzes them. It's a powerful tool, often used in conjunction with Graphite to create meaningful graphs based on log analysis.
* Splunk - Enterprisey

### PaaS
* Loggly
* Airbrake.io
* Sumplogic - Enterprisey
* Papertrail
* Logentries
* Splunk Storm - SaaS for Splunk

## Lastly
Here's an interesting idea on logging: Fingers Crossed logging is the idea of only saving logs which are near errors. If logs do not appear near errors, they are (eventually) discarded.  
For PHP readers, Monolog has a ton of handlers which may clue you into other services or ideas on how to handle logging.  
For AWS users, Elastic Load Balancers will finally have Access logs and other data available!  

Reference: https://serversforhackers.com/c/all-about-logs