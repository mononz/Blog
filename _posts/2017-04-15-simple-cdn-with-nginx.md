---
layout: post
title:  Simple CDN with nginx
date:   2017-04-15 20:51:00
categories: Linux
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---

Nginx is a high performance webserver. This post will run through a simple installation of nginx on CentOS 7. Refer to my getting started post [here](/2016-04-14-centos-7-getting-started) for creating a base CentOS 7 install with a super user.

### 1. Add Nginx repo

Add the CentOS 7 EPEL repository

```
sudo yum install epel-release
```

### 2. Install and Start Nginx

Install and start nginx with

```
sudo yum install nginx
sudo systemctl start nginx
```

__Configure firewall rules__

If your server is running a firewall, run the following to allow http and https traffic

```
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```

Test that Nginx is running by visiting your website

> http://YOUR_SERVER_IP/

You should see something like

<img src="/assets/content/nginx/nginx_default.png" alt="nginx" style="width:80%">

__Make sure you enable nginx on system startup__

```
sudo systemctl enable nginx
```

### 3. Nginx config

It's time to start serving out own content!

The default server root directory is located at `/usr/share/nginx/html`. Files placed there are will be served up by nginx.


__Static Webpage Example__

Overwrite `/usr/share/nginx/html/index.html` with the following content (Simple webpage that says 'Hi.'). The existing file just shows the basic 'Welcome to nginx screen' (like the pic above) which we should get rid of anyway.

```
<!DOCTYPE html>
<html>
<head>
<style>
body {
  font-family: sans-serif;
  font-size: 50px;
  width: 100%;
  height: 100%;
  margin: 0;
}

div {
  position:absolute;
  height:100%;
  width:100%;
  display: table;
}

h1 {
  display: table-cell;
  vertical-align: middle;
  text-align:center;
}
</style>
</head>
<body>
  <div>
  <h1>Hi.</h1>
  </div>
</body>
</html>
```

Now restart nginx

```
sudo systemctl restart nginx
```

Visit your website to see the new landing page

> http://YOUR_SERVER_IP/

You should see something like

<img src="/assets/content/nginx/nginx_test.png" alt="nginx" style="width:80%">


__Simple CDN Example__

To start modifying the config, create/edit entries in `/etc/nginx/conf.d/`.

Create the file `/etc/nginx/conf.d/static.mononz.net.conf` with the content (Sub in your own url). Any files ending in .conf will be picked up by nginx.

```
server {
  listen 80;
  server_name static.mononz.net;

  location / {
     expires 7d;
     root /home/admin/static;
  }
}
```

Test your nginx config using

```
sudo nginx -t
```

If test shows ok/successful, restart nginx

```
sudo systemctl restart nginx
```

This Nginx config will serve files located in the admin users directory 'static'. Note: You can push files up to your server easily by using rsync

```
rsync -avz ~/Desktop/static/ admin@SERVER_IP_ADDRESS:static/
```

If you had a file in your `static/` folder named `image.png`. You should be able to see your file via the url

> http://YOUR_SERVER_IP/image.png

### Bonus - https

Want to live in the https world? Checkout [Cloudflares flexible ssl](https://www.cloudflare.com/ssl/). It's free, automatically enabled and too easy!
