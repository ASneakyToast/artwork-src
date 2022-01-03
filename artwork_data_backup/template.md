---
title: null
date: 0001-01-01
size: (400px, 600px)
description: none
flavor-text:
- none
alt-text: none
flavor-text: none
series: none
collections:
- Junior-Review
tags:
- sculpture
- printmaking

original-file: jpg
cover-photo: true
pin-amount: 1
card-amount: 0
featured: false
published: false
---

This is the excerpt; everything above the more html like comment;
<!--more-->
Full amount of content below this.

### Related Content
<nuxt-link to="/articles">Nuxt Link to Blog</nuxt-link>

<a href="/artcles">Html Link to Blog</a>

[Markdown Link to Blog](/articles)

<a href="https://nuxtjs.org">External link html</a>

[External Link markdown](https://nuxtjs.org)

### Code Blocks
Use these for super cool text
```js{1,3-5}[server.js]
const http = require('http')
const bodyParser = require('body-parser')

http.createServer(( req, res ) => {
  bodyParser.parse( req, ( error, body ) => {
    res.end( body )
  })
}).listen( 3000 )
```

