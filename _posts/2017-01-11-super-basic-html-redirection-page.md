---
layout: post
title:  Super basic html redirection page w/ google analytics
date:   2013-11-10 10:18:00
categories: Web
---

```
<!DOCTYPE HTML>

<meta charset="UTF-8">
<meta http-equiv="refresh" content="1; url=http://example.com">

    <script>
        (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
                    (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
                m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
        })(window,document,'script','//www.google-analytics.com/analytics.js','ga');
        ga('create', 'UA-XXXXXXXX-X', 'auto');
        ga('send', 'pageview');
    </script>

<script>
  window.location.href = "https://play.google.com/store/apps/developer?id=mononz"
</script>

<title>Page Redirection</title>

<!-- Note: don't tell people to `click` the link, just tell them that it is a link. -->
If you are not redirected automatically, follow <a href='https://play.google.com/store/apps/developer?id=mononz'>this link</a>
```
