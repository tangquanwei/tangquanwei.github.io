---
title: Docker Draw.io
tags: 
- Docker 
- Draw.io
date: 2023-07-07 22:23:45
cover:
---
## Docker 安装
```bash
docker run -it -m1g \
  -v "/home/quanwei/.drawio/letsencrypt-log:/var/log/letsencrypt/" \
  -v "/home/quanwei/.drawio/letsencrypt-etc:/etc/letsencrypt/" \
  -v "/home/quanwei/.drawio/letsencrypt-lib:/var/lib/letsencrypt" \
  -e LETS_ENCRYPT_ENABLED=true \
  -e PUBLIC_DNS=drawio.example.com \
  --name="drawio" \
  -p 8088:8080 \
  jgraph/drawio
```

## 启动

Start a web browser session to http://localhost:8080/?offline=1&https=0 

If you're running Docker Toolbox then start a web browser session to
 http://192.168.99.100:8080/?offline=1&https=0