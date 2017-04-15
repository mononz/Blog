---
layout: post
title:  Ubuntu Custom Launcher (Ex. Teamspeak3)
date:   2016-06-09 04:16:41
categories: Linux
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---


Some software such as Teamspeak3 can be rather annoying to install on Linux OS's such as Ubuntu.

Teamspeak comes downloaded as a .run file. To install, you'd have to run the commands:

```language-bash
  $ chmod a+x teamspeak.run
  $ ./teamspeak.run
```

After which you can run the program using terminal with:

```language-bash
  $ sh /Teamspeak/ts3client_runscript.sh
```

Ideally, on a windows system during install we get a start menu entry. On Ubuntu however, we do not. Easiest way I found was to create a Ubuntu Desktop Entry

#### Create a new Desktop Entry

Create a file called Teamspeak.desktop and paste the following in (sub in your values where appropriate).

```language-bash
#!/usr/bin/env xdg-open

[Desktop Entry]
Version=3.0.16  
Name=Teamspeak3
GenericName=Teamspeak
Exec=/home/jared/Software/Teamspeak3/ts3client_runscript.sh
Terminal=false
X-MultipleArgs=false
Type=Application
Icon=/home/jared/Software/Teamspeak3/icon.png
StartupWMClass=Teamspeak
StartupNotify=true
```

Then mark it as executable using the terminal command

```language-bash
  $ chmod +x Teamspeak.desktop
```

Should now be able to double click to open :)
