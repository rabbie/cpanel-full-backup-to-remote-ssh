cpanel-full-backup-to-remote-ssh
================================

This script creates a backup of all cpanel accounts on your local machine and then copies each backup to a remote backup server via SSH.

This is particularly useful for servers that do not have an equal amount of disk space available as is used on the server. i.e, if your /home directory is 50GB, you need another 50GB (approx) in order to keep your cPanel managed backups. This script will create a backup -> copy the backup to the remote server -> delete the backup from the local server -> move on

Cron
====

```
1 5 * * * /root/scripts/dailybackup.sh
```

The File Itself
===============

```
REMOTEUSER="backup-user"
REMOTESERVER="backupserver1.mydomain.com"
REMOTEBACKUPDIR="~/backups/cpanelServer/" # must end in a trailing slash

for i in `find /home/ -maxdepth 1 -type d -name [^\.]\* | sed 's:^\./::' | awk -F"/" '{print $3}'`;do /scripts/pkgacct $i /backup/; scp /backup/* $REMOTEUSER@$REMOTESERVER:$REMOTEBACKUPDIR`date -I` ;rm -f /backup/*;done
