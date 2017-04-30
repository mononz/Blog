---
layout: post
title:  SSH Agent Forwarding
date:   2017-04-30 12:00:00
categories: Linux
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---

Use git on your server and already setup with ssh for git authentication on your local machine? Great, this guide is for you!

We can use a technique called SSH Agent forwarding which will use our local machines ssh key on the remote server. It makes for easy deployments and doesn't leave keys (without passphrases) sitting on your server.

Firstly, check your current ssh keys in your agent

``` shell
$ ssh-agent -l
```

Got some there? great! If not add your keys using

``` shell
$ ssh-add ~/.ssh/id_rsa
```

__Aside: Help! macOS keep forgetting my ssh keys__

Mac will forget our ssh-keys on each reboot... Fun times... Alas! starting in macOS Sierra, Apple has added the UseKeychain config option for SSH configs to solve this. Let's enable that by adding the following to the file `~/.ssh/config` (Doesn't exist? create it)

``` plaintext
Host *
  UseKeychain yes
```

### 1. Test local key

If you get the message below, you are good to go!

``` shell
$ ssh -T git@github.com
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

Else, check that your key in your agent is actually linked to your github account and retry.

### 2. Set up for SSH agent forwarding

Open up the file `~/.ssh/config` (Doesn't exist? create it) and enter the following

``` plaintext
Host example.com
  ForwardAgent yes
```

__DO NOT__ use a wildcard like `Host *`.  You really wouldn't want to provide your local SSH keys with every server you shell into hey! __Only add servers you trust here!__

### 3. Test SSH agent forwarding

ssh into your server and run that git@github.com command again

``` shell
$ ssh user@example.com
$ ssh -T git@github.com
```

You should get the same prompt as before

> Hi username! You've successfully authenticated, but GitHub does not provide shell access.

This means that your ssh key was forwarded across and not authenticates with github! cool aye! Clone, pull whatever to your hearts content.

If you get the result 

> Permission denied (publickey).` 

something went wrong. Repeat the above and see what you might have missed.

### Reference

Check out the official github post [here](https://developer.github.com/guides/using-ssh-agent-forwarding/) for troubleshooting and gotchas