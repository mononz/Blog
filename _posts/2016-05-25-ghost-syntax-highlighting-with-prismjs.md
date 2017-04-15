---
layout: post
title:  Ghost syntax highlighting with Prismjs
date:   2016-05-25 02:44:04
categories: Ghost
---

Currently, I like the default casper theme in ghost. Its minimal, focusses on the content and does the job I want. One thing however is missing, syntax highlighting for code snippets. I did a bit of research and found [Prism](http://prismjs.com/). It is lightweight, simple and looks great!

```language-sql
  // demo: bit of raw sql
  SELECT * FROM users WHERE name='mononz';
```

This shouldn't take more than 5-10mins to setup.

# Install Prism

- Jump over to http://prismjs.com/download.html
- Select a theme and the languages you would like to support
- Scroll down to the bottom and find the `prism.js` and `prism.css` files. You can download them or just copy paste the content later.
- Log into your server and navigate to your theme directory (In my case - casper).

```language-bash
  $ cd ghost/content/themes/casper
```

- Copy the `prism.js` file to the `assets/js/prism.js` path
- Copy the `prism.css` file to the `assets/css/prism.css` path
- Edit the default.hbs file in your current theme directory adding a link to the css at the top

```language-markup
...
  {{!-- Styles'n'Scripts --}}
  <link rel="stylesheet" type="text/css" href="{{asset "css/screen.css"}}" />
  {{!-- Prism.js --}}
  <link rel="stylesheet" type="text/css" href="{{asset "css/prism.css"}}" />
  {{!-- Font --}}
  <link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Merriweather:300,700,700italic,300italic|Open+Sans:700,400" />
...
```

 and a script to the js at the bottom

```language-markup
...
  {{!-- The main JavaScript file for Casper --}}
  <script type="text/javascript" src="{{asset "js/index.js"}}"></script>
  {{!-- Prism.js --}}
  <script type="text/javascript" src="{{asset "js/prism.js"}}"></script>
...
```

And thats it! You are good to go. Start generating content with the new syntax highlighting like so

```language-markdown
  ```language-markup
    <h1>Hello!!</h1>  
  ```
```

which will render as

```language-markup
  <h1>Hello!!</h1>  
```

Swap out the 'markup' for anything you ticked on the prism download page such as javascript, sql, markdown, bash, c ...


#### Note

It won't preview in the side panel. You'll have to manually preview your post :)
