---
layout: post
title:  Google Compute Cloud - Investigation
date:   2016-05-07 13:23:40
categories: Linux
---

I recently started looking at using Google Compute Cloud for quickly deploying projects without having to worry about server configuration constantly. Here's what I found.

## Why the investigation?

I have been using Digital Ocean for a couple of years now, but I have had this lingering concern over upscaling and monitoring.

After releasing the directives feature in my PSArchives Android app, the node api on the $5/mo Debian droplet began peaking at 100% and occasionally restarting. To admit, there is a bit of low performance code in there, but hey, I'm still learning and there are plenty of optimisations available to try.

That aside, I began to worry about what happens if the app got very popular suddenly, I need to upsize my server quickly to cope with demand. Also, with unoptimised code and resources potentially being maxed out I want to know when to upgrade.

Post is currently being written, come back later :)

...
