---
title: Nginx에서 static file serving하기
date: 2019-09-01 01:09:52
category: troubleshooting
---

```shell
  server {
      listen       80;
      server_name  localhost;

      charset utf-8;
      location / {
          root   /usr/share/nginx/html;
          index  index.html index.htm;
          try_files $uri /index.html?query_string;
      }

  }
```
