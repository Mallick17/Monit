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



