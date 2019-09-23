> https://code.visualstudio.com/docs/remote/troubleshooting#_configuring-key-based-authentication

---

先查看本地是否已经有公钥了，如果没有则生成：

```
ssh-keygen -t rsa -b 4096
```

如果已经有了，则直接负责到远程主机即可：

```
$ ls ~/.ssh/id_rsa.pub
/Users/fuxiaosong/.ssh/id_rsa.pub

$ ssh-copy-id root@182.61.24.127
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/fuxiaosong/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@182.61.24.127's password:

Number of key(s) added:        1

Now try logging into the machine, with:   "ssh 'root@182.61.24.127'"
and check to make sure that only the key(s) you wanted were added.
```
