# How to clean all log files in CWP – Control WebPanel – to gain DISK Space

In this tutorial I’ll explain and provide solution script upon how you can clear all log files including old logs from CWP server. This tutorial is specially dedicated to the blog visitors who have emailed me to provide such solution.

This script will clean all possible logs without affecting any service. Ensure you’ve logged in as root ssh user

## Solution 1 :

To clear the log instantly you can run this bash script as i already created for your to make the task faster or else if you want to create by your own go to solution 2.
```
curl -s -L https://www.uxlinux.com/upload/clear-sh.sh | bash
```
## Solution 2 :

First create a file in /root dir :  
```
cd /root
nano clearlog.sh
```
Then paste this Bash script and save it:

```
#!/bin/bash

truncate -s 0  /usr/local/apache/logs/*bytes
truncate -s 0  /usr/local/apache/logs/*log
truncate -s 0  /usr/local/apache/domlogs/*bytes
truncate -s 0  /usr/local/apache/domlogs/*log
truncate -s 0 /var/log/messages
truncate -s 0 /var/log/maillog
truncate -s 0 /var/log/*log
truncate -s 0 /opt/alt/*/usr/var/log/php-fpm.log
truncate -s 0  /usr/local/cwpsrv/logs/access_log
truncate -s 0  /usr/local/cwpsrv/logs/error_log
truncate -s 0  /var/log/cron
truncate -s 0  /var/log/secure
truncate -s 0  /var/log/cwp/services_action.log
truncate -s 0  /var/log/cwp/cwp_sslmod.log
truncate -s 0  /var/log/cwp/cwp_cron.log
truncate -s 0  /var/log/cwp/cwp_backup.log
truncate -s 0  /var/log/cwp/activity.log
truncate -s 0  /usr/local/cwpsrv/var/services/roundcube/logs/errors
truncate -s 0  /var/spool/amavisd/.razor/razor-agent.log
truncate -s 0  /usr/local/cwp/php71/var/log/php-fpm.log
truncate -s 0  /root/.acme.sh/cwp_certs/acme.sh.log
rm -rf /var/log/maillog-*
rm -rf /var/log/monit.log-*
rm -rf /var/log/spooler-*
rm -rf /var/log/messages-*
rm -rf /var/log/secure-*
rm -rf /var/log/pureftpd.log-*
rm -rf /var/log/yum.log-*
rm -rf /var/log/monit.log-*
rm -rf /var/log/cron-*
rm -rf /var/lib/clamav/tmp.*
```

**Change the permission:**

```
chmod 755 /root/clearlog.sh
```

**Now run this command to run the clear log script:**
```
sh /root/clearlog.sh
```

Thats it the logs will be cleared you can go and check to the respected locations of the log.

## Cron Job :

You can also create cron job like below by following solution 2 and create this cron job task according to your needs :

**TO run DAILY:**
```
0 0 * * * /usr/bin/sh /root/clearlog.sh
```

**TO run Weekly :**
```
0 0 * * 0 /usr/bin/sh /root/clearlog.sh
```

**TO run Monthly:**
```
0 0 1 * * /usr/bin/sh /root/clearlog.sh
```
