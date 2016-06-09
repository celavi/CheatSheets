# CCZE and Multitail

## CCZE (A robust log colorizer)

### How-To Color Highlight Logs with tailf
```bash
tailf kern.log | ccze -A
```
### How-To Color Highlight Logs with tailf and less
```bash
tailf kern.log | ccze -A | less -R
```
## Multitail

### Automatically add new files to a window
```bash
multitail -Q /var/log/myprogram/log-2014*.txt
```
### Merge 2 logfiles in one window:
```bash
multitail /var/log/apache/access.log -I /var/log/apache/error.log
```
### Show 3 logfiles in 2 columns:
```bash
multitail -s 2 /var/log/apache/access.log /var/log/messages /var/log/mail.log
```
### Show 5 logfiles while merging 2 and put them in 2 columns with only one in the left column:
```bash
multitail -s 2 -sn 1,3  /var/log/apache/access.log -I /var/log/apache/error.log /var/log/messages \
                        /var/log/mail.log /var/log/syslog
```
### Merge the output of 2 ping commands while removing "64 bytes received from" from only 1 of them:
```bash
multitail -l "ping 192.168.0.1" -ke "64 bytes from" -L "ping 192.168.0.2"
```
### Show the output of a ping-command and if it displays a timeout, send a message to all users currently logged in:
```bash
multitail -ex timeout "echo timeout | wall" -l "ping 192.168.0.1"
```
### In one window show all new TCP connections and their state changes using netstat while in the other window displaying the merged access and error logfiles of apache:
```bash
multitail -R 2 -l "netstat -t" /var/log/apache/access.log -I /var/log/apache/error.log
```
### As the previosu example but also copy the output to the file netstat.log:
```bash
multitail -a netstat.log -R 2 -l "netstat -t tcp" /var/log/apache/access.log -I /var/log/apache/error.log
```
### Show 2 logfiles merged in one window but give each logfile a different color so that you can easily see what lines are for what logfile:
```bash
multitail -ci green /var/log/apache/access.log -ci red -I /var/log/apache/error.log
```
### Show 3 rssfeeds merged in one window using rsstail:
```bash
multitail -cS rsstail -l "rsstail -n 1 -z -l -d -u http://setiathome.berkeley.edu/rss_main.php" \
    -cS rsstail -L "rsstail -n 1 -z -l -d -u http://www.biglumber.com/index.rss" -cS rsstail \
    -L "rsstail -n 1 -z -l -u http://kernel.org/kdist/rss.xml"
```
### Show a Squid (proxy server) logfile while converting timestamps to something readable:
```bash
multitail -cv squid /var/log/squid/access.log
```
### Display Q-Mail logging while converting the timestamp into human readable format:
```bash
multitail -cv qmailtimestr /var/log/qmail/qmail.smtpd.log
```
### Merge ALL apache logfiles (*access_log/*error_log) into one window:
```bash
multitail -cS apache --mergeall /var/log/apache/*access_log --no-mergeall -cS apache_error \
    --mergeall /var/log/apache/*error_log --no-mergeall
```
### Merge 2 logfiles in one window, but give different color to each logfile so that you can easily understand what lines are for what logfile:
```bash
multitail -ci green /var/log/yum.log -ci yellow -I /var/log/mysqld.log
```
