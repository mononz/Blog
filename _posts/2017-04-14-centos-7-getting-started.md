---
layout: post
title:  CentOS 7 - Getting started
date:   2017-04-14 05:38:19
categories: Linux
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---

Provision a new VPS with CentOS 7. [Vultr](http://www.vultr.com/?ref=7079263) is awesome and very affordable, do check it out if you haven't already.

### 1. Login

Using ssh, connect to your new VPS with ssh
```
$ ssh root@YOUR_SERVER_IP
```

### 2. New User

Root has increased privileges to do nearly everything on your server. And because of this, its not a good idea at all to use it as your regular user. We need to create a regular user who can use increased privileges only when it needs to.

Create a user with the following command (make sure you are currently logged in as root)
```
# adduser steve
```
Add a strong password for the new user 'steve'
```
# passwd steve
```

Sometimes we need to administrate our server. We can avoid having logging in as root by adding the new user to 'super user' group. This way the user can run administrative actions by using the word 'sudo' before a command. In CentOS 7, users should be part of the 'wheel' group.

As root, run the command
```
# gpasswd -a steve wheel
```

### 3. Public Key (Suggested)

To secure the our user and the server more so, we should require a private SSH key to log in.

If you have a key setup already, skip to 'Copy public key'

**Generate keys**

On your local machine run
```
$ ssh-keygen
```
Accept all, or add a new name (I called mine vultr)

**Copy Public key**

Can simply use the 'ssh-copy-id' command to copy our new key on our local machine to the remote server. Keys will be added to the remote users '.ssh/authorized_keys' file.

```
$ ssh-copy-id -i ~/.ssh/vultr.pub steve@YOUR_SERVER_IP
```

Try an ssh login to your server
```
$ ssh steve@YOUR_SERVER_IP
```

You no longer should require a password to login as your local and remote keys allow you entry.

**Can't login?**
Ensure your key is added to your ssh-agent on your local machine
```
$ ssh-add -K ~/.ssh/vultr
```

### 4. SSH Daemon

Now that the server is a bit more secure, let's disable remote root ssh login. We can always use our user to escalate up to root when required

```
# nano /etc/ssh/sshd_config
```

Look for the line
```
#PermitRootLogin yes
```

and change it to
```
PermitRootLogin no
```

now reload ssh
```
# systemctl reload sshd
```

### 5. Party!

You are all done!
