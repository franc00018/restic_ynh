You should now allow the following public key on the target server `__SERVER__` :

- User: `__SSH_USER__`
- Public key: `__PUBLIC_KEY__`

Do so by running those commands on `__SERVER__` with user `__SSH_USER__`  :

```
mkdir ~/.ssh -p
touch ~/.ssh/authorized_keys
chmod u=rw,go= ~/.ssh/authorized_keys
echo "__PUBLIC_KEY__" >> ~/.ssh/authorized_keys
```

Also make sure `__BACKUP_PATH__` exists and is writable by `__SSH_USER__`
