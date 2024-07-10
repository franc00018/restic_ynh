### Configuring the user on the target server

You should now authorize the public key on the target server using something like:

```
mkdir ~/.ssh -p
touch ~/.ssh/authorized_keys
chmod u=rw,go= ~/.ssh/authorized_keys
cat << EOPKEY >> ~/.ssh/authorized_keys
<paste here the private key displayed at the end of installation>
EOPKEY
```

Optional: to improve security, make sure the user can only connect through SFTP and can only access its home directory on the target server.
On Debian/Ubuntu, this is done using the following snippet. Otherwise refer to your distribution manual (don't forget to replace `servera` with the real username)

```
cat << EOF>> /etc/ssh/sshd_config
Match User servera
   ChrootDirectory %h
   ForceCommand internal-sftp
   AllowTcpForwarding no
   X11Forwarding no
EOF
systemctl restart ssh
```

### Test

At this step your backup should schedule.

If you want to be sure, you can test it by running on server A:
```
systemctl start restic.service
```

Next you can verify the backup contents by running on server A
```
restic -r sftp:serverb.domain.tld:servera.domain.tld/auto_conf snapshots
```

Replace `auto_conf` with `auto_<app>` if you did not choose to backup configuration but only applications.

If you want to check the backups consistency:
```
systemctl start restic_check.service
```

If you want to make a complete check of the backups - keep in mind that this reads all the backed up data, it can take some time depending on your target server upload speed (more on this topic in [the Restic documentation](https://restic.readthedocs.io/en/latest/045_working_with_repos.html#checking-integrity-and-consistency)):
```
systemctl start restic_check_read_data.service
```

### Misc helpful commands

- Display the apps list to backup: `yunohost app setting restic apps`
- Edit the apps list to backup: `yunohost app setting restic apps -v "nextcloud,wordpress"`
- Launch a backup: `systemctl start restic`
- Launch a backup check: `systemctl start restic_check.service`

##### Launch a complete backup check

WARNING: this will read data from your backups destination server.
It may take a quite long time depending on the target server's internet upload speed and hardware performance.

```
systemctl start restic_check_read_data.service
```
