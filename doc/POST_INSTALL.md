You should now allow the following public key for user __SSH_USER__ on server __SERVER__:

__PUBLIC_KEY__

Do so by running those commands on __SERVER__ with user __SSH_USER__:

```
mkdir ~/.ssh -p
touch ~/.ssh/authorized_keys
chmod u=rw,go= ~/.ssh/authorized_keys
echo "__PUBLIC_KEY__" >> ~/.ssh/authorized_keys
```

Also make sure __BACKUP_PATH__ exists and is writable by __SSH_USER__

If you're facing an issue or want to improve this app, please open a new issue in this project: <https://github.com/YunoHost-Apps/restic_ynh>
