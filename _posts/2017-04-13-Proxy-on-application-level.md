---
layout : post
comments : true
title : Proxy on application level
---

Basically proxy is handled on system level. It's easy enough to use if you can modify port mappings on your server. However, as you know, it's too restricted to edit configs in servers if you're working for a company. You don't have to depend on the system configuration. You can setup proxies on application level. This is how it works.

<!--break-->

The main idea is using extensibility of HTTP headers. You can add any extended headers on your requests and responses with *X-* prefix. Although [*X-* prefixed headers is deprecated](https://tools.ietf.org/html/rfc6648) since June, 2012, it's still effective. In addition, I don't think this extension will disappear.

First, prepare a table representing proxy key to destination id to decide the request destination. For example:

| Proxy Key | Destination ID |
| 1         | 40001          |
| 2         | 40002          |
| 3         | 40003          |

Then, have a proxy server which induces requests to the designated endpoints based on the table we made. If you're using apache, you can configure like this:
```bash
[dyong@ip-172-31-25-124 helloweb]$ cat /etc/httpd/conf.d/proxy.conf
RewriteEngine On
RewriteCond %{HTTP:X-Proxy-Key} ^.+
RewriteRule ^ http://localhost:4%{HTTP:X-Proxy-Key}%{REQUEST_URI} [P,QSA,L]
```

And let's create a simple web app. This app is going be spawned by Docker, ann run on a Docker container behind the proxy server.

```bash
[dyong@ip-172-31-25-124 helloweb]$ cat Dockerfile
FROM mhart/alpine-node:base-6

WORKDIR /app

COPY . .

EXPOSE 8080

CMD ["node", "index.js"]
```

```bash
[dyong@ip-172-31-25-124 helloweb]$ cat index.js
var express = require('express')
var app = express()
var os = require('os')

app.get('/', function (req, res) {
        res.send('Hi, I am from container' + os.hostname())
})

app.listen(8080)

```

```bash
[dyong@ip-172-31-25-124 helloweb]$ cat package.json
{
  "name": "helloweb",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "author": "",
  "license": "MIT",
  "dependencies": {
    "express": "^4.15.2"
  }
}

```

Now we all prepared the recipes. Let's spawn the app 3 times on each container.

```bash
[dyong@ip-172-31-25-124 helloweb]$ sudo docker run -d -p 40001:8080 helloweb
40b369ceac6750734ca6bdba7cee2606e90916f1390994537ee932ef38f4b968
[dyong@ip-172-31-25-124 helloweb]$ sudo docker run -d -p 40002:8080 helloweb
65c6788d19362436d3d0a2af345b317606bd13effc949727bbbd2736ef63653d
[dyong@ip-172-31-25-124 helloweb]$ sudo docker run -d -p 40003:8080 helloweb
945a71ffe71f4b6aba508f4b97772c5aa41be2babd608b2b7bcae2b5a6e02173
```

If you make some request on the proxy server with specific proxy key, you wlil get the responses from the server with exactly matched key:

```bash
for key in {0001..0003}; do curl -H "X-Proxy-Key:${key}" -w "\n" http://ec2-13-113-86-240.ap-northeast-1.compute.amazonaws.com; done
Hi, I am from container40b369ceac67
Hi, I am from container65c6788d1936
Hi, I am from container945a71ffe71f
```

```bash
ssh dyong@ec2 'sudo docker ps --format "{{.ID}} {{.Image}} {{.Ports}}"'
40b369ceac67 helloweb 0.0.0.0:40001->8080/tcp
945a71ffe71f helloweb 0.0.0.0:40003->8080/tcp
65c6788d1936 helloweb 0.0.0.0:40002->8080/tcp
```
