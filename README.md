# M/Monit
- Monit is a small Open Source utility for managing and monitoring Unix systems.
- M/Monit is a system for automatic management and pro-active monitoring of Information Technology Systems. M/Monit can monitor and manage distributed computer systems, conduct automatic maintenance and repair and execute meaningful causal actions in error situations.
- M/Monit uses Monit as an agent and can manage and monitor all your hosts and services. M/Monit can start a service if it does not run, restart a service if it does not respond and suspend a service if it uses too much resources.
- Monitor system attributes such as CPU, Load, Memory, Disk usage, Files, Directories and Filesystems for changes on all your hosts. Conditional rules can be set and if a value goes outside a defined scope, specific actions can be executed and a notification sent.
- M/Monit is a modern, compact and scalable application server. Thread-pools and a non-blocking, event driven i/o architecture is used to ensure high performance. M/Monit runs on any POSIX system and uses minimal system resources.
- Database access is handled by a connection pool with support for MySQL, MariaDB, PostgreSQL and SQLite.

### SYSTEM REQUIREMENTS
- **Memory and Disk space**: A minimum of 10 megabytes of RAM is required, along with approximately 25 MB of free disk space. You may need more RAM depending on the number of monitored hosts and services, how many processor threads the M/Monit server is started with, and the number of active login sessions.
- **CPU requirements**: No special requirements. A low-end system with a single x86_64 CPU should be able to provide enough power to manage hundreds of Monit agents and hundreds of M/Monit web-app users.
- **Network communication requirements**: M/Monit communicate with Monit agents on TCP port 2812. If there is a NAT or PAT (port translation) between M/Monit and Monit, you will need to setup host information in M/Monit so M/Monit can connect to Monit over the network. This can be specified in the admin/hosts page in M/Monit. Otherwise M/Monit will use the host information it receive from Monit when Monit automatically registered itself in M/Monit.
  ![image](https://github.com/user-attachments/assets/3c0862c6-7da0-4e4f-af65-8845adfee9b2)

### Architecture Overview
![image](https://github.com/user-attachments/assets/7d6eba5e-ea60-4bfa-adae-bab5c2850719)
- Monit is a small, powerful monitoring program that runs on each host monitored by M/Monit. With regular intervals, Monit will send a message to M/Monit with the status of the host it is running on. If a service fails or Monit has to perform an action to fix a problem, an event message is sent to M/Monit at once. Both status and event messages are stored in a database. Upon receiving an event message from Monit, M/Monit will consult its rule-set and perform an alert notification if a rule matched.
- From M/Monit, you can start, stop and restart services on any of your hosts running Monit.

### Benefits
- Your computer systems will have a higher uptime as M/Monit can handle error conditions automatically, often without the need for human intervention.
- M/Monit has a clean, simple and well designed user interface which scales well, if you manage 2 hosts or 1000+ hosts.

### Setup and Configuration in Ubuntu Server 22.04 LTS (HVM)
- This document provides a step-by-step guide to installing, configuring, and managing Monit on a Linux system.

#### 1. Update the Package List

First, update the package list to ensure you have the latest information on available packages.

```bash
sudo apt update
```

#### 2. Install Monit

Install Monit using the `apt` package manager.

```bash
sudo apt install monit -y
```

#### 3. Check Monit Service Status

After installation, check the status of the Monit service to ensure it is running.

```bash
sudo systemctl status monit.service
```

#### 4. Navigate to the Monit Configuration Directory

Change to the Monit configuration directory.

```bash
cd /etc/monit/
```

#### 5. List Directory Contents

List the contents of the Monit configuration directory to verify the presence of the `monitrc` file.

```bash
ls
```

#### 6. Edit the Monit Configuration File

Open the `monitrc` file for editing using the `vi` editor.

```bash
sudo vi monitrc
```

#### 7. Enable Monit Service

Enable the Monit service to start automatically on system boot.

```bash
sudo systemctl enable monit.service
```

#### 8. Check Monit Service Status Again

Verify that the Monit service is enabled and running.

```bash
sudo systemctl status monit.service
```

#### 9. Check Monit Status

Check the status of Monit to ensure it is functioning correctly.

```bash
sudo monit status
```

#### 10. Edit the Monit Configuration File Again

Reopen the `monitrc` file to make specific configuration changes.

```bash
sudo vi monitrc
```

##### Line 159: Edit the HTTPD Configuration

Modify the `monitrc` file to set up the Monit web interface. Uncomment and edit the following lines:

```bash
set httpd port 2812 and
    use address 0.0.0.0  # only accept connection from localhost (drop if you use M/Monit)
    allow 0.0.0.0/0        # allow localhost to connect to the server and
    allow admin:monit      # require user 'admin' with password 'monit'
#    #with ssl {            # enable SSL/TLS and set path to server certificate
#    #    pemfile: /etc/ssl/certs/monit.pem
#    #}
```
<details>
  <summary>Click to view complete `monitrc` configuration file</summary>

```bash
root@ip-172-31-6-184:/etc/monit# cat monitrc
###############################################################################
## Monit control file
###############################################################################
##
## Comments begin with a '#' and extend through the end of the line. Keywords
## are case insensitive. All path's MUST BE FULLY QUALIFIED, starting with '/'.
##
## Below you will find examples of some frequently used statements. For
## information about the control file and a complete list of statements and
## options, please have a look in the Monit manual.
##
##
###############################################################################
## Global section
###############################################################################
##
## Start Monit in the background (run as a daemon):
#
set daemon  30             # check services at 120 seconds intervals
#   with start delay 240    # optional: delay the first check by 4-minutes (by
#                           # default Monit check immediately after Monit start)
#
#
## Set syslog logging. If you want to log to a standalone log file instead,
## specify the full path to the log file
#
set log /var/log/monit.log

#
#
## Set the location of the Monit lock file which stores the process id of the
## running Monit instance. By default this file is stored in $HOME/.monit.pid
#
# set pidfile /var/run/monit.pid
#
## Set the location of the Monit id file which stores the unique id for the
## Monit instance. The id is generated and stored on first Monit start. By
## default the file is placed in $HOME/.monit.id.
#
# set idfile /var/.monit.id
set idfile /var/lib/monit/id
#
## Set the location of the Monit state file which saves monitoring states
## on each cycle. By default the file is placed in $HOME/.monit.state. If
## the state file is stored on a persistent filesystem, Monit will recover
## the monitoring state across reboots. If it is on temporary filesystem, the
## state will be lost on reboot which may be convenient in some situations.
#
# set statefile /var/.monit.state
set statefile /var/lib/monit/state
#
#

## Set limits for various tests. The following example shows the default values:
##
# set limits {
#     programOutput:     512 B,      # check program's output truncate limit
#     sendExpectBuffer:  256 B,      # limit for send/expect protocol test
#     fileContentBuffer: 512 B,      # limit for file content test
#     httpContentBuffer: 1 MB,       # limit for HTTP content test
#     networkTimeout:    5 seconds   # timeout for network I/O
#     programTimeout:    300 seconds # timeout for check program
#     stopTimeout:       30 seconds  # timeout for service stop
#     startTimeout:      30 seconds  # timeout for service start
#     restartTimeout:    30 seconds  # timeout for service restart
# }

## Set global SSL options (just most common options showed, see manual for
## full list).
#
# set ssl {
#     verify     : enable, # verify SSL certificates (disabled by default but STRONGLY RECOMMENDED)
#     selfsigned : allow   # allow self signed SSL certificates (reject by default)
# }
#
#
## Set the list of mail servers for alert delivery. Multiple servers may be
## specified using a comma separator. If the first mail server fails, Monit
## will use the second mail server in the list and so on. By default Monit uses
## port 25 - it is possible to override this with the PORT option.
#
# set mailserver mail.bar.baz,               # primary mailserver
#                backup.bar.baz port 10025,  # backup mailserver on port 10025
#                localhost                   # fallback relay
#
#
## By default Monit will drop alert events if no mail servers are available.
## If you want to keep the alerts for later delivery retry, you can use the
## EVENTQUEUE statement. The base directory where undelivered alerts will be
## stored is specified by the BASEDIR option. You can limit the queue size
## by using the SLOTS option (if omitted, the queue is limited by space
## available in the back end filesystem).
#
 set eventqueue
     basedir /var/lib/monit/events  # set the base directory where events will be stored
     slots 100                      # optionally limit the queue size
#
#
## Send status and events to M/Monit (for more information about M/Monit
## see https://mmonit.com/). By default Monit registers credentials with
## M/Monit so M/Monit can smoothly communicate back to Monit and you don't
## have to register Monit credentials manually in M/Monit. It is possible to
## disable credential registration using the commented out option below.
## Though, if safety is a concern we recommend instead using https when
## communicating with M/Monit and send credentials encrypted. The password
## should be URL encoded if it contains URL-significant characters like
## ":", "?", "@". Default timeout is 5 seconds, you can customize it by
## adding the timeout option.
#
# set mmonit http://monit:monit@192.168.1.10:8080/collector
#     # with timeout 30 seconds              # Default timeout is 5 seconds
#     # and register without credentials     # Don't register credentials
#
#
## Monit by default uses the following format for alerts if the mail-format
## statement is missing::
## --8<--
## set mail-format {
##   from:    Monit <monit@$HOST>
##   subject: monit alert --  $EVENT $SERVICE
##   message: $EVENT Service $SERVICE
##                 Date:        $DATE
##                 Action:      $ACTION
##                 Host:        $HOST
##                 Description: $DESCRIPTION
##
##            Your faithful employee,
##            Monit
## }
## --8<--
##
## You can override this message format or parts of it, such as subject
## or sender using the MAIL-FORMAT statement. Macros such as $DATE, etc.
## are expanded at runtime. For example, to override the sender, use:
#
# set mail-format { from: monit@foo.bar }
#
#
## You can set alert recipients whom will receive alerts if/when a
## service defined in this file has errors. Alerts may be restricted on
## events by using a filter as in the second example below.
#
# set alert sysadm@foo.bar                       # receive all alerts
#
## Do not alert when Monit starts, stops or performs a user initiated action.
## This filter is recommended to avoid getting alerts for trivial cases.
#
# set alert your-name@your.domain not on { instance, action }
#
#
## Monit has an embedded HTTP interface which can be used to view status of
## services monitored and manage services from a web interface. The HTTP
## interface is also required if you want to issue Monit commands from the
## command line, such as 'monit status' or 'monit restart service' The reason
## for this is that the Monit client uses the HTTP interface to send these
## commands to a running Monit daemon. See the Monit Wiki if you want to
## enable SSL for the HTTP interface.
#
set httpd port 2812 and
    use address 0.0.0.0  # only accept connection from localhost (drop if you use M/Monit)
    allow 0.0.0.0/0        # allow localhost to connect to the server and
    allow admin:monit      # require user 'admin' with password 'monit'
#    #with ssl {            # enable SSL/TLS and set path to server certificate
#    #    pemfile: /etc/ssl/certs/monit.pem
#    #}
#
## Monit can perform act differently regarding services previous state when
## going back in duty. By default, Monit will 'start' all services. Monit can
## also takes no action to start services in 'nostart' mode. Monit can try to
## restore the 'laststate' of the service when Monit was shutdown.
# set onreboot start # start, nostart, laststart

###############################################################################
## Services
###############################################################################
##
## Check general system resources such as load average, cpu and memory
## usage. Each test specifies a resource, conditions and the action to be
## performed should a test fail.
#
#  check system $HOST
#    if loadavg (1min) per core > 2 for 5 cycles then alert
#    if loadavg (5min) per core > 1.5 for 10 cycles then alert
#    if cpu usage > 95% for 10 cycles then alert
#    if memory usage > 75% then alert
#    if swap usage > 25% then alert
#
#
## Check if a file exists, checksum, permissions, uid and gid. In addition
## to alert recipients in the global section, customized alert can be sent to
## additional recipients by specifying a local alert handler. The service may
## be grouped using the GROUP option. More than one group can be specified by
## repeating the 'group name' statement.
#
#  check file apache_bin with path /usr/local/apache/bin/httpd
#    if failed checksum and
#       expect the sum 8f7f419955cefa0b33a2ba316cba3659 then unmonitor
#    if failed permission 755 then unmonitor
#    if failed uid "root" then unmonitor
#    if failed gid "root" then unmonitor
#    alert security@foo.bar on {
#           checksum, permission, uid, gid, unmonitor
#        } with the mail-format { subject: Alarm! }
#    group server
#
#
## Check that a process is running, in this case Apache, and that it respond
## to HTTP and HTTPS requests. Check its resource usage such as cpu and memory,
## and number of children. If the process is not running, Monit will restart
## it by default. In case the service is restarted very often and the
## problem remains, it is possible to disable monitoring using the TIMEOUT
## statement. This service depends on another service (apache_bin) which
## is defined above.
#
#  check process apache with pidfile /usr/local/apache/logs/httpd.pid
#    start program = "/etc/init.d/httpd start" with timeout 60 seconds
#    stop program  = "/etc/init.d/httpd stop"
#    if cpu > 60% for 2 cycles then alert
#    if cpu > 80% for 5 cycles then restart
#    if totalmem > 200.0 MB for 5 cycles then restart
#    if children > 250 then restart
#    if disk read > 500 kb/s for 10 cycles then alert
#    if disk write > 500 kb/s for 10 cycles then alert
#    if failed host www.tildeslash.com port 80 protocol http and request "/somefile.html" then restart
#    if failed port 443 protocol https with timeout 15 seconds then restart
#    if 3 restarts within 5 cycles then unmonitor
#    depends on apache_bin
#    group server
#
#
## Check filesystem permissions, uid, gid, space usage, inode usage and disk I/O.
## Other services, such as databases, may depend on this resource and an automatically
## graceful stop may be cascaded to them before the filesystem will become full and data
## lost.
#
#  check filesystem datafs with path /dev/sdb1
#    start program  = "/bin/mount /data"
#    stop program  = "/bin/umount /data"
#    if failed permission 660 then unmonitor
#    if failed uid "root" then unmonitor
#    if failed gid "disk" then unmonitor
#    if space usage > 80% for 5 times within 15 cycles then alert
#    if space usage > 99% then stop
#    if inode usage > 30000 then alert
#    if inode usage > 99% then stop
#    if read rate > 1 MB/s for 5 cycles then alert
#    if read rate > 500 operations/s for 5 cycles then alert
#    if write rate > 1 MB/s for 5 cycles then alert
#    if write rate > 500 operations/s for 5 cycles then alert
#    if service time > 10 milliseconds for 3 times within 5 cycles then alert
#    group server
#
#
## Check a file's timestamp. In this example, we test if a file is older
## than 15 minutes and assume something is wrong if its not updated. Also,
## if the file size exceed a given limit, execute a script
#
#  check file database with path /data/mydatabase.db
#    if failed permission 700 then alert
#    if failed uid "data" then alert
#    if failed gid "data" then alert
#    if timestamp > 15 minutes then alert
#    if size > 100 MB then exec "/my/cleanup/script" as uid dba and gid dba
#
#
## Check directory permission, uid and gid.  An event is triggered if the
## directory does not belong to the user with uid 0 and gid 0.  In addition,
## the permissions have to match the octal description of 755 (see chmod(1)).
#
#  check directory bin with path /bin
#    if failed permission 755 then unmonitor
#    if failed uid 0 then unmonitor
#    if failed gid 0 then unmonitor
#
#
## Check a remote host availability by issuing a ping test and check the
## content of a response from a web server. Up to three pings are sent and
## connection to a port and an application level network check is performed.
#
#  check host myserver with address 192.168.1.1
#    if failed ping then alert
#    if failed port 3306 protocol mysql with timeout 15 seconds then alert
#    if failed port 80 protocol http
#       and request /some/path with content = "a string"
#    then alert
#
#
## Check a network link status (up/down), link capacity changes, saturation
## and bandwidth usage.
#
#  check network public with interface eth0
#    if link down then alert
#    if changed link then alert
#    if saturation > 90% then alert
#    if download > 10 MB/s then alert
#    if total uploaded > 1 GB in last hour then alert
#
#
## Check custom program status output.
#
#  check program myscript with path /usr/local/bin/myscript.sh
#    if status != 0 then alert
#
#
###############################################################################
## Includes
###############################################################################
##
## It is possible to include additional configuration parts from other files or
## directories.
#
#  include /etc/monit.d/*
include /etc/monit/conf.d/*
include /etc/monit/conf-enabled/*
#
```
  
</details>

#### 11. Restart Monit Service

Restart the Monit service to apply the configuration changes.

```bash
sudo systemctl restart monit
```

#### 12. Check Monit Status Again

Verify that Monit is running with the new configuration.

```bash
sudo monit status
```

#### 13. Accessing the Monit Web Interface
After completing the setup, you can access the Monit web interface by navigating to `http://13.233.154.166:2812/` in your web browser. You will be prompted to enter the `username (admin)` and `password (monit)` as configured in the monitrc file.
![image](https://github.com/user-attachments/assets/12e843c6-fa55-42e1-adb5-c28c8066c7be)

## Explanation of `/etc/monit/conf.d` and Configuration of Monit for Monitoring a Running Process & Send E-Mail Alerts if it Fails

The `/etc/monit/conf.d` directory is where Monit stores its configuration files. Monit uses these files to define how it monitors and manages services on your system. Each file in this directory typically corresponds to a specific service or process that Monit is monitoring.

In your case, the `nginx` file in `/etc/monit/conf.d` contains the configuration for monitoring the Nginx web server. Here's a breakdown of the configuration:

```plaintext
check process nginx with pidfile /run/nginx.pid
          start program = "/usr/sbin/service nginx start" with timeout 60 seconds
          stop program = "/usr/sbin/service nginx stop"
          if failed host 127.0.0.1 port 80 protocol http then alert
```

- **`check process nginx with pidfile /run/nginx.pid`**: Monit checks the Nginx process using the PID file located at `/run/nginx.pid`.
- **`start program = "/usr/sbin/service nginx start" with timeout 60 seconds`**: If Monit detects that Nginx is not running, it will attempt to start it using the specified command.
- **`stop program = "/usr/sbin/service nginx stop"`**: If Monit needs to stop Nginx, it will use this command.
- **`if failed host 127.0.0.1 port 80 protocol http then alert`**: Monit will check if Nginx is responding to HTTP requests on `localhost` (port 80). If it fails, Monit will trigger an alert.

1. **Create a Configuration File**:
   Create a new file in `/etc/monit/conf.d/` for the process you want to monitor. For example, to monitor Nginx:

   ```bash
   sudo nano /etc/monit/conf.d/nginx
   ```

2. **Add the Monitoring Configuration**:
   Add the following configuration to monitor Nginx:

   ```plaintext
   check process nginx with pidfile /run/nginx.pid
       start program = "/usr/sbin/service nginx start" with timeout 60 seconds
       stop program = "/usr/sbin/service nginx stop"
       if failed host 127.0.0.1 port 80 protocol http then alert
   ```

3. **Reload Monit**:
   Reload Monit to apply the new configuration:

   ```bash
   sudo monit reload
   ```

4. **Verify Monitoring**:
   Check the status of the monitored process:

   ```bash
   sudo monit status
   ```

   You should see the status of the Nginx process and any alerts if it fails.

---

---
# Configure Monit to send Mail Alerts

### Setting Up Monit to Send Email Alerts

- To configure Monit to send email alerts, you need to set up the mail server settings in the main Monit configuration file (`/etc/monit/monitrc`). Below is a step-by-step guide to configure email alerts:

#### 1. **Edit the Monit Configuration File**:
   Open the `/etc/monit/monitrc` file in a text editor:

   ```bash
   sudo nano /etc/monit/monitrc
   ```
<details>
  <summary>Click to view complete `monitrc` configuration file to send Mail Alerts</summary>

```ini
root@ip-172-31-6-26:/etc/monit# cat monitrc
###############################################################################
## Monit control file
###############################################################################
##
## Comments begin with a '#' and extend through the end of the line. Keywords
## are case insensitive. All path's MUST BE FULLY QUALIFIED, starting with '/'.
##
## Below you will find examples of some frequently used statements. For
## information about the control file and a complete list of statements and
## options, please have a look in the Monit manual.
##
##
###############################################################################
## Global section
###############################################################################
##
## Start Monit in the background (run as a daemon):
#
set daemon  10             # check services at 120 seconds intervals
#   with start delay 240    # optional: delay the first check by 4-minutes (by
#                           # default Monit check immediately after Monit start)
#
#
## Set syslog logging. If you want to log to a standalone log file instead,
## specify the full path to the log file
#
set log /var/log/monit.log

#
#
## Set the location of the Monit lock file which stores the process id of the
## running Monit instance. By default this file is stored in $HOME/.monit.pid
#
# set pidfile /var/run/monit.pid
#
## Set the location of the Monit id file which stores the unique id for the
## Monit instance. The id is generated and stored on first Monit start. By
## default the file is placed in $HOME/.monit.id.
#
# set idfile /var/.monit.id
set idfile /var/lib/monit/id
#
## Set the location of the Monit state file which saves monitoring states
## on each cycle. By default the file is placed in $HOME/.monit.state. If
## the state file is stored on a persistent filesystem, Monit will recover
## the monitoring state across reboots. If it is on temporary filesystem, the
## state will be lost on reboot which may be convenient in some situations.
#
# set statefile /var/.monit.state
set statefile /var/lib/monit/state
#
#

## Set limits for various tests. The following example shows the default values:
##
# set limits {
#     programOutput:     512 B,      # check program's output truncate limit
#     sendExpectBuffer:  256 B,      # limit for send/expect protocol test
#     fileContentBuffer: 512 B,      # limit for file content test
#     httpContentBuffer: 1 MB,       # limit for HTTP content test
#     networkTimeout:    5 seconds   # timeout for network I/O
#     programTimeout:    300 seconds # timeout for check program
#     stopTimeout:       30 seconds  # timeout for service stop
#     startTimeout:      30 seconds  # timeout for service start
#     restartTimeout:    30 seconds  # timeout for service restart
# }

## Set global SSL options (just most common options showed, see manual for
## full list).
#
# set ssl {
#     verify     : enable, # verify SSL certificates (disabled by default but STRONGLY RECOMMENDED)
#     selfsigned : allow   # allow self signed SSL certificates (reject by default)
# }
#
#
## Set the list of mail servers for alert delivery. Multiple servers may be
## specified using a comma separator. If the first mail server fails, Monit
## will use the second mail server in the list and so on. By default Monit uses
## port 25 - it is possible to override this with the PORT option.
#
 set mailserver smtp.gmail.com port 587
      username "gyanaranjanmallick444@gmail.com" password "fxbx jcxt brvi rihz"
      using tls

 set alert gyanaranjanmallick17@gmail.com
#
#
## By default Monit will drop alert events if no mail servers are available.
## If you want to keep the alerts for later delivery retry, you can use the
## EVENTQUEUE statement. The base directory where undelivered alerts will be
## stored is specified by the BASEDIR option. You can limit the queue size
## by using the SLOTS option (if omitted, the queue is limited by space
## available in the back end filesystem).
#
 set eventqueue
     basedir /var/lib/monit/events  # set the base directory where events will be stored
     slots 100                      # optionally limit the queue size
#
#
## Send status and events to M/Monit (for more information about M/Monit
## see https://mmonit.com/). By default Monit registers credentials with
## M/Monit so M/Monit can smoothly communicate back to Monit and you don't
## have to register Monit credentials manually in M/Monit. It is possible to
## disable credential registration using the commented out option below.
## Though, if safety is a concern we recommend instead using https when
## communicating with M/Monit and send credentials encrypted. The password
## should be URL encoded if it contains URL-significant characters like
## ":", "?", "@". Default timeout is 5 seconds, you can customize it by
## adding the timeout option.
#
# set mmonit http://monit:monit@192.168.1.10:8080/collector
#     # with timeout 30 seconds              # Default timeout is 5 seconds
#     # and register without credentials     # Don't register credentials
#
#
## Monit by default uses the following format for alerts if the mail-format
## statement is missing::
## --8<--
 set mail-format {
   from:    Monit <monit@$HOST>
   subject: monit alert --  $EVENT $SERVICE
   message: $EVENT Service $SERVICE
                 Date:        $DATE
                 Action:      $ACTION
                 Host:        $HOST
                 Description: $DESCRIPTION

            Your faithful employee,
            Monit
 }
## --8<--
##
## You can override this message format or parts of it, such as subject
## or sender using the MAIL-FORMAT statement. Macros such as $DATE, etc.
## are expanded at runtime. For example, to override the sender, use:
#
# set mail-format { from: monit@foo.bar }
#
#
## You can set alert recipients whom will receive alerts if/when a
## service defined in this file has errors. Alerts may be restricted on
## events by using a filter as in the second example below.
#
# set alert sysadm@foo.bar                       # receive all alerts
#
## Do not alert when Monit starts, stops or performs a user initiated action.
## This filter is recommended to avoid getting alerts for trivial cases.
#
# set alert your-name@your.domain not on { instance, action }
#
#
## Monit has an embedded HTTP interface which can be used to view status of
## services monitored and manage services from a web interface. The HTTP
## interface is also required if you want to issue Monit commands from the
## command line, such as 'monit status' or 'monit restart service' The reason
## for this is that the Monit client uses the HTTP interface to send these
## commands to a running Monit daemon. See the Monit Wiki if you want to
## enable SSL for the HTTP interface.
#
set httpd port 2812 and
    use address 172.31.6.26  # only accept connection from localhost (drop if you use M/Monit)
    allow 0.0.0.0/0        # allow localhost to connect to the server and
    allow admin:monit      # require user 'admin' with password 'monit'
#    #with ssl {            # enable SSL/TLS and set path to server certificate
#    #    pemfile: /etc/ssl/certs/monit.pem
#    #}
#
## Monit can perform act differently regarding services previous state when
## going back in duty. By default, Monit will 'start' all services. Monit can
## also takes no action to start services in 'nostart' mode. Monit can try to
## restore the 'laststate' of the service when Monit was shutdown.
# set onreboot start # start, nostart, laststart

###############################################################################
## Services
###############################################################################
##
## Check general system resources such as load average, cpu and memory
## usage. Each test specifies a resource, conditions and the action to be
## performed should a test fail.
#
#  check system $HOST
#    if loadavg (1min) per core > 2 for 5 cycles then alert
#    if loadavg (5min) per core > 1.5 for 10 cycles then alert
#    if cpu usage > 95% for 10 cycles then alert
#    if memory usage > 75% then alert
#    if swap usage > 25% then alert
#
#
## Check if a file exists, checksum, permissions, uid and gid. In addition
## to alert recipients in the global section, customized alert can be sent to
## additional recipients by specifying a local alert handler. The service may
## be grouped using the GROUP option. More than one group can be specified by
## repeating the 'group name' statement.
#
#  check file apache_bin with path /usr/local/apache/bin/httpd
#    if failed checksum and
#       expect the sum 8f7f419955cefa0b33a2ba316cba3659 then unmonitor
#    if failed permission 755 then unmonitor
#    if failed uid "root" then unmonitor
#    if failed gid "root" then unmonitor
#    alert security@foo.bar on {
#           checksum, permission, uid, gid, unmonitor
#        } with the mail-format { subject: Alarm! }
#    group server
#
#
## Check that a process is running, in this case Apache, and that it respond
## to HTTP and HTTPS requests. Check its resource usage such as cpu and memory,
## and number of children. If the process is not running, Monit will restart
## it by default. In case the service is restarted very often and the
## problem remains, it is possible to disable monitoring using the TIMEOUT
## statement. This service depends on another service (apache_bin) which
## is defined above.
#
#  check process apache with pidfile /usr/local/apache/logs/httpd.pid
#    start program = "/etc/init.d/httpd start" with timeout 60 seconds
#    stop program  = "/etc/init.d/httpd stop"
#    if cpu > 60% for 2 cycles then alert
#    if cpu > 80% for 5 cycles then restart
#    if totalmem > 200.0 MB for 5 cycles then restart
#    if children > 250 then restart
#    if disk read > 500 kb/s for 10 cycles then alert
#    if disk write > 500 kb/s for 10 cycles then alert
#    if failed host www.tildeslash.com port 80 protocol http and request "/somefile.html" then restart
#    if failed port 443 protocol https with timeout 15 seconds then restart
#    if 3 restarts within 5 cycles then unmonitor
#    depends on apache_bin
#    group server
#
#
## Check filesystem permissions, uid, gid, space usage, inode usage and disk I/O.
## Other services, such as databases, may depend on this resource and an automatically
## graceful stop may be cascaded to them before the filesystem will become full and data
## lost.
#
#  check filesystem datafs with path /dev/sdb1
#    start program  = "/bin/mount /data"
#    stop program  = "/bin/umount /data"
#    if failed permission 660 then unmonitor
#    if failed uid "root" then unmonitor
#    if failed gid "disk" then unmonitor
#    if space usage > 80% for 5 times within 15 cycles then alert
#    if space usage > 99% then stop
#    if inode usage > 30000 then alert
#    if inode usage > 99% then stop
#    if read rate > 1 MB/s for 5 cycles then alert
#    if read rate > 500 operations/s for 5 cycles then alert
#    if write rate > 1 MB/s for 5 cycles then alert
#    if write rate > 500 operations/s for 5 cycles then alert
#    if service time > 10 milliseconds for 3 times within 5 cycles then alert
#    group server
#
#
## Check a file's timestamp. In this example, we test if a file is older
## than 15 minutes and assume something is wrong if its not updated. Also,
## if the file size exceed a given limit, execute a script
#
#  check file database with path /data/mydatabase.db
#    if failed permission 700 then alert
#    if failed uid "data" then alert
#    if failed gid "data" then alert
#    if timestamp > 15 minutes then alert
#    if size > 100 MB then exec "/my/cleanup/script" as uid dba and gid dba
#
#
## Check directory permission, uid and gid.  An event is triggered if the
## directory does not belong to the user with uid 0 and gid 0.  In addition,
## the permissions have to match the octal description of 755 (see chmod(1)).
#
#  check directory bin with path /bin
#    if failed permission 755 then unmonitor
#    if failed uid 0 then unmonitor
#    if failed gid 0 then unmonitor
#
#
## Check a remote host availability by issuing a ping test and check the
## content of a response from a web server. Up to three pings are sent and
## connection to a port and an application level network check is performed.
#
#  check host myserver with address 192.168.1.1
#    if failed ping then alert
#    if failed port 3306 protocol mysql with timeout 15 seconds then alert
#    if failed port 80 protocol http
#       and request /some/path with content = "a string"
#    then alert
#
#
## Check a network link status (up/down), link capacity changes, saturation
## and bandwidth usage.
#
#  check network public with interface eth0
#    if link down then alert
#    if changed link then alert
#    if saturation > 90% then alert
#    if download > 10 MB/s then alert
#    if total uploaded > 1 GB in last hour then alert
#
#
## Check custom program status output.
#
#  check program myscript with path /usr/local/bin/myscript.sh
#    if status != 0 then alert
#
#
###############################################################################
## Includes
###############################################################################
##
## It is possible to include additional configuration parts from other files or
## directories.
#
#  include /etc/monit.d/*
include /etc/monit/conf.d/*
include /etc/monit/conf-enabled/*
#
```

</details>

### 2.Configure GMail to send mail alerts from `monit`
#### App Password
- If you have 2-Step Verification enabled on your Google account, you cannot use your regular Google account password. Instead, you need to generate an **App Password** specifically for M/Monit.
  
<details>
  <summary>Click to view CONFIGURATION of GMAIL to SEND MAIL ALERTS through M/MONIT</summary>

#### 1. **Generate an App Password (if 2-Step Verification is enabled)**:
   - Go to your [Google Account](https://myaccount.google.com/).
   - Navigate to **Security** > **2-Step Verification** (enable it if not already enabled).
   - Scroll down to **App Passwords** and generate a new app password.
   - Use this app password in your USE NAME AS `M/Monit` configuration instead of your regular Gmail password.

#### 2. **Update M/Monit Configuration**:
   Update your M/Monit configuration file (`monitrc`) with the correct credentials. For example:

   ```plaintext
   set mailserver smtp.gmail.com port 587
       username "gyanaranjanmallick444@gmail.com" password "your_app_password"
       using tls
   ```

   Replace `your_app_password` with the appropriate password (app password if 2-Step Verification is enabled).

#### 3. **Restart M/Monit**:
   After updating the configuration, restart M/Monit to apply the changes:

   ```bash
   sudo monit restart
   ```

#### 4. **Test the Configuration**:
   Send a test alert to verify that the email notifications are working.

---

### Additional Notes:
- If you're using a firewall, ensure that outbound traffic on port 587 is allowed in the security group.

</details>

#### 3. **Configure the Mail Server**:
   Add the following lines to configure the mail server settings. Replace the placeholders with your actual email credentials:

   ```plaintext
   set mailserver smtp.gmail.com port 587
       username "your_email@gmail.com" password "your_app_password"
       using tls
   ```

   - **`smtp.gmail.com`**: The SMTP server for Gmail.
   - **`port 587`**: The port for TLS encryption.
   - **`username`**: Your Gmail address.
   - **`password`**: Your Gmail password or app password (if 2-Step Verification is enabled).
   - **`using tls`**: Enables TLS encryption for secure communication.

#### 4. **Set the Email Recipient**:
   Specify the email address where alerts should be sent:

   ```plaintext
   set alert your_email@gmail.com
   ```

   Replace `your_email@gmail.com` with the email address you want to receive alerts.

#### 5. **Configure Email Format (Optional)**:
   You can customize the email format by adding the following lines:

   ```plaintext
   set mail-format {
       from: monit@yourdomain.com
       subject: Monit Alert -- $EVENT $SERVICE
       message: Monit $ACTION $SERVICE at $DATE on $HOST: $DESCRIPTION.
   }
   ```

   - **`from`**: The sender's email address.
   - **`subject`**: The subject line of the email.
   - **`message`**: The body of the email.
   - Save the changes and exit the text editor.

#### 6. **Reload Monit**:
   After making changes to the configuration, reload Monit to apply the new settings:

   ```bash
   sudo monit reload
   ```

#### 7. **Test the Configuration**:
   To test if email alerts are working, you can manually trigger an alert. For example, stop the Nginx service and see if Monit sends an email:

   ```bash
   sudo service nginx stop
   ```

   Monit should detect that Nginx is down and send an email alert to the specified address.

---

### Documentation Summary

1. **`/etc/monit/conf.d`**: Directory containing Monit configuration files for individual services.
2. **Email Alerts**:
   - Configure the mail server in `/etc/monit/monitrc`.
   - Use Gmail's SMTP server with TLS encryption.
   - Set the recipient email address for alerts.
3. **Monitoring a Process**:
   - Create a configuration file in `/etc/monit/conf.d/` for the process.
   - Define start/stop commands and failure conditions.
   - Reload Monit to apply changes.

