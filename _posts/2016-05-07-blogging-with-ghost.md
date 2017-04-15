---
layout: post
title:  Blogging with Ghost
date:   2016-05-07 13:44:06
categories: Ghost
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---

I've been meaning to try out https://ghost.org/ for a long time and I finally found a spare moment install it on my vps and give it a proper try. First impression... amazing!

## Last blog failure

My last attempt at a blog was using [docpad](https://docpad.org/) which was ok, but I could never bother to sit down, write content, generate the html, and then deploy it to a server. Ghost has a insanely easy web interface that took no effort to learn and I can write content on my phone on the bus. Too easy!

I mainly kept my last blog going while I was looking for work to make myself look keen and smart. Once I got a job, I didn't bother to maintain it and I soon pulled it down in favour of a redirect to my [Google Play Developer](https://play.google.com/store/apps/dev?id=5292198157762057076) profile.

## Self Installation

The entire process should take less than 15 mins providing you have:

* A VPS ready to go -> preferably Debian/Ubuntu based.
* A non-root user with sudo privileges.
* And a domain pointed to your VPS IP

I ran into issues with NodeJS version 6.0.0, The ghost seemed to require node v4.2.0 so I installed node using [Node Version Manager](https://github.com/creationix/nvm) (aka nvm)

##### 1 - Install Node/NPM

First up, lets ensure our system is up to date and some packages are installed

```language-bash
    $ sudo apt-get update
    $ sudo apt-get install wget zip
```

Then install nvm with

```language-bash
    $ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
    $ source ~/.bashrc
```

Test nvm is available with

```language-bash
    $ nvm --version
```

Now lets install node version 4.2.0

```language-bash
    $ nvm install 4.2.0
    $ nvm use 4.2.0
```

and check its available with

```language-bash
    $ node -v
    $ npm -v
```

this should list `v4.2.0` and `2.14.7` respectively in which case we are good to go!

##### 2 - Install Ghost

I just installed ghost into the current user home directory `/home/blog/` but I guess you could put it anywhere. Start by downloading the latest ghost and unzipping it

```language-bash
    $ wget https://ghost.org/zip/ghost-latest.zip
    $ unzip -d ghost ghost-latest.zip
    $ cd ghost
```

Just quickly, because we are running on production, I usually set the NODE_ENV environment variable to production. Saves using the `--production` flag all the time. I do this by appending `export NODE_ENV=production` to the top line of `~/.bashrc` and reloading it

```language-bash
    $ nano ~/.bashrc
    $ source ~/.bashrc
```

once that's all done, we can install ghost with npm

```language-bash
    $ npm install --production
```

If no errors, ghost is installed and ready to go!

##### 3 - Configure Ghost

We have to configure our ghost blog before we can start bloggin

```language-bash
    $ cp config.example.js config.js
    $ nano config.js
```

You then must change the url in the production section from `url: 'http://my-ghost-blog.com',` to your domain name so the links in the blog work correctly. In my case it was `url: 'http://mononz.net',` but you could use an ip address if your domain is not setup yet like `url: 'http://146.148.41.26',`

Also update the host in the server section from `host: '127.0.0.1',` to `host: '0.0.0.0',`.

I also changed the post from `2368` to `3000` due to specific server config for myself and firewall rules.

Save the file and run ghost with

```language-bash
    $ npm start --production
```

If no errors, you should be able to check out your new blog at http://www.mononz.net:3000 (or http://146.148.41.26:3000) substituting your domain, ip address and port appropriately.

##### 4 - Keep Ghost up forever

I use [pm2](https://github.com/Unitech/pm2) for uptime but others also use [forever](https://www.npmjs.com/package/forever). For this guide I am using pm2 as I am most familiar with it.

Install pm2 globally with npm and start your ghost with

```language-bash
    $ npm install pm2 -g
    $ pm2 start index.js --name "blog"
```

Should now have a process running like so

<img src="/assets/content/ghost/pm2.png" alt="pm2 list" style="width:80%">

and you should be able to still see your site running on http://www.mononz.net:3000

Lets make pm2 automatically bring up our website if the server ever goes offline. Pm2 has a simple command called startup

```language-bash
    $ pm2 startup
```

You'll be given a command to run as sudo as per below. Do that command and you'll be all sorted for your blog to startup with your server!

<img src="/assets/content/ghost/pm2-startup.png" alt="pm2 startup" style="width:80%">

Only one step away from finishing our setup

##### 4 - Setup Nginx

Put simply, nginx allows us to not have to append the port number every time so we can access our site using http://www.mononz.net  rather than http://www.mononz.net:3000.

Install nginx using the command

```language-bash
    $ sudo apt-get install nginx
```

Lets open the nginx folder where we can remove the default config and add in our new config

```language-bash
    $ cd /etc/nginx
    $ sudo rm sites-enabled/default
    $ sudo nano /etc/nginx/sites-available/ghost
```

Copy in the following text swapping out my server_name for yours

```language-nginx
    server {
        listen 80;
        server_name mononz.net www.mononz.net;
        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
```

now symlink our config from `sites-available` to `sites-enabled` with

```language-bash
    $ sudo ln -s /etc/nginx/sites-available/ghost /etc/nginx/sites-enabled/ghost
```

restart nginx with

```language-bash
    $ sudo service nginx restart
```

You are all done! Check out your blog at your domain like http://www.mononz.net and follow the setup instructions to get started!

## Final thoughts

I am loving ghost at the moment, seems like a great piece of software and I look forward to writing blog posts more frequently with new things I learn. Give it a go yourself and don't forget to donate back to ghost if you too love it.

Cheers!!
