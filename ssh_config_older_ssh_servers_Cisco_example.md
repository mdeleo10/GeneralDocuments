## How to configure ssh client on Mac/Linux to connect to older Cisco routers and switches or even outdated hosts

Error that appears:

```
mdeleo@mdeleo: ssh 192.168.10.253

Unable to negotiate with 192.168.10.253 port 22: no matching key exchange method found. Their offer: diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1
```

## Using command line parameters

```
mdeleo@mdeleo:~$ ssh -o KexAlgorithms="diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1"  192.168.10.253
```

--------

## Configuring .ssh/config for each remote host

```
vi .ssh/config
Host 192.168.10.253
  KexAlgorithms +diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
```


Example:

```
Host 192.168.10.1
  KexAlgorithms +diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
  Ciphers aes128-cbc
  HostKeyAlgorithms=+ssh-dss

Host 192.168.10.97
  KexAlgorithms +diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
  Ciphers aes128-cbc

Host 192.168.10.98
  KexAlgorithms +diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
  Ciphers aes128-cbc

Host 192.168.10.234
  KexAlgorithms +diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1

Host 192.168.10.253
  KexAlgorithms +diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
  Ciphers aes128-cbc

Host 192.168.65.1
  KexAlgorithms +diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
  Ciphers aes128-cbc

Host 2006:240:5C0:35::1
  KexAlgorithms +diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
  Ciphers aes128-cbc

Host 192.168.65.253
  KexAlgorithms +diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
  Ciphers aes128-cbc

```

--------

Note: Tested on Mac and Ubuntu Linux
 
--------

If you need to keep sessions alive, put this in the beginning of you "config" file

```
# https://linux-tips.com/t/how-to-keep-ssh-sessions-alive/255
Host *
  ServerAliveInterval 6s0
  ServerAliveCountMax 3
```
