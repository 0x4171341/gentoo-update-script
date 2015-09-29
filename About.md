# About #

Being a old OpenBSD user, I have grown quite accustom to receiving the daily email outputs from the fantastic /etc/daily, /etc/weekly, and /etc/month cronjobs. Now that I am supporting several Gentoo based servers, I find myself longing for that same system maintenance automation.

To addressed this, I have created a shell script for Gentoo to preform various nightly system administration tasks from a cron job and then email me a report reminiscent of OpenBSD's /etc/daily reports. This script is generic enough to run on all of my Gentoo based boxes. Additionally, since most of the servers I support serve some sort of security function, I've included optional auto-updating for Nikto plugins, Snort signatures, and Nessus plugins.

This script is released AS-IS under the New BSD License (http://www.opensource.org/licenses/bsd-license.html) and is available from my Google Code page (http://code.google.com/p/gentoo-update-script/). While I am currently running this script in production environments, it should still be considered Beta. Please feel free to change/add/ improve as you see fit.

**Install:**

---


-Copy update-script.sh to the /etc/cron.daily/ directory on your Gentoo box.
-Optionally, to avoid having all Gentoo boxes attempt to update at the same time, edit /etc/crontab to reflect when the cron.daily jobs should run.

**Modify Backup Retention:**

---


The variable BACK\_LNG determines how long you want to keep backups.

**Optional SSH/rsync backup:**

---


To enable rsync usage with SSH first create a key, copy it to the remote system.
```
$ ssh-keygen -t dsa
$ ssh-copy-id -i ~/.ssh/id_dsa.pub ssh-user@remotehost.com
$ ssh ssh-user@remotehost.com
$ mkdir /backups/hostname -f
$ sudo chown ssh-user /backups/hostname.domain.com/
```
Next you need to modify the variables for the rsync including enabling it, setting remote domain, remote user, key on local system, and if desired directory to exclude from backup.

**Optional MySQL Backup:**

---


To enable MySQL backup, set the user, password, and host of the MySQL database. Secondly set the MySQL variable to yes. Each database will be backed up separately.

**Optional Backup Encryption:**

---


To enable AES-256 bit encryption of backups which is disabled by default, set the ENCRYPT variable = "yes".  Next set the ENC\_PASS variable to the password of your choosing.  Note that since this script will now hold your hardcoded backup passwords, please exercise proper caution when setting file permissions.

To decrypt your archived backup, issue the command:

openssl enc -d -aes-256-cbc -in 2007-07-30.etc.backup.tar.gz.enc -out 2007-07-30.etc.backup.tar.gz

Substituting in the proper filenames and enter your password when prompted.

**Optional Portage Cleanup:**

---


You may enable the script to remove old portage files. Note the default value is 90 days but this may be adjusted as needed.

**Caveats:**

---


-It appears that alot of automated jobs run on the hour to update Nessus signatures.  Because of this, updates can sometimes fail.  In an attempt to avoid this, the script contains a seventeen (17) minute sleep timer before the Nessus signature updates trigger.  Please feel free to adjust accordingly.
-Backups need incorporate offbox storage to be true backups.  I usually accomplish this via Samba or NSF mounting a /backups directory from another machine.  The primary purpose of the backup job is just to protect me in the event that a file is accidentally overwritten (etc-update).

**Changelog:**

---

Visit the readme.txt file for a accurate list of all changes.

**To-Do/ Wishlist:**

---

-Check for and use a static configuration file to override program defaults.






