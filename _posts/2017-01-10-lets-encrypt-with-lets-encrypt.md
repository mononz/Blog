---
layout: post
title:  Let's Encrypt with Let's Encrypt
date:   2017-01-08 06:30:04
categories: Linux
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---

SSL'ing up my sites has always been on my todo list. I decided to make my own 'cdn' using **nginx** on one of my **debian** servers and thought I really should secure it.

I'm going to be using [letsencrypt](https://letsencrypt.org/) and [certbot](https://certbot.eff.org/) to get the job done. I am using debian(Jesse) + Nginx so I am following instructions from [here](https://certbot.eff.org/#debianjessie-nginx)

#### Generate with certbot

Install the certbot package from Jessie backports. If you don't know how to use backports, read [here](https://backports.debian.org/Instructions/)
```
sudo apt-get install certbot -t jessie-backports
```

I'm going to use single cmdline method specifying the webroot and domain name.

```
certbot certonly --webroot -w /home/mononz/static -d static.mononz.net
```

You should get a nice response like...

```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/static.mononz.net/fullchain.pem. Your cert
   will expire on 2017-04-08. To obtain a new or tweaked version of
   this certificate in the future, simply run certbot again. To
   non-interactively renew *all* of your certificates, run "certbot
   renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

After obtaining the cert, you will have the following PEM-encoded files:

 - cert.pem: Your domain's certificate
 - chain.pem: The Let's Encrypt chain certificate
 - fullchain.pem: cert.pem and chain.pem combined
 - privkey.pem: Your certificate's private key

The will live in `/etc/letsencrypt/archive` BUT they are symlinked nicely for us at `/etc/letsencrypt/live/static.mononz.net`

#### Increased security

To further increase security, you should also generate a strong Diffie-Hellman group. To generate a 2048-bit group, use this command:

```
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

which created a file at `/etc/ssl/certs/dhparam.pem`

#### nginx setup

open up the file at `nano /etc/nginx/sites-available/static.mononz.net`. Edit it to be like mine. (REMEMBER, I am securing a basic CDN with nginx so your config might be very different)

```
server {
  listen 443 ssl;

  server_name static.mononz.net;

  ssl_certificate /etc/letsencrypt/live/static.mononz.net/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/static.mononz.net/privkey.pem;

  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;
  ssl_dhparam /etc/ssl/certs/dhparam.pem;
  ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
  ssl_session_timeout 1d;
  ssl_session_cache shared:SSL:50m;
  ssl_stapling on;
  ssl_stapling_verify on;
  add_header Strict-Transport-Security max-age=15768000;

  location / {
    expires 7d;
    root /home/mononz/static;
  }
}

server {
  listen 80;
  server_name static.mononz.net;
  return 301 https://$host$request_uri;
}
```
the second server block will redirect all regular traffic down the secured https route.

Check your nginx config using

```
sudo nginx -t
```

If all is good, do a nginx service restart
```
sudo service nginx restart
```

Use a web browser to check your security like
```
https://www.ssllabs.com/ssltest/analyze.html?d=static.mononz.net
```
You should have an A or A+

#### Remember

Let’s Encrypt certificates are valid for 90 days. The certbot comes with a cron to auto renew them.

You should check auto renewal by doing a dry run.

```
certbot renew --dry-run
```

And to confirm, loading up http://static.mononz.net/planetside/84832.png auto redirects me to https://static.mononz.net/planetside/84832.png

<figure>
<img src="/assets/content/lets_encrypt/secure.png" alt="LastEpisode" style="width:100%">
<figcaption><center>AWESOME!</center></figcaption>
</figure>
