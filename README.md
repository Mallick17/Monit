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

