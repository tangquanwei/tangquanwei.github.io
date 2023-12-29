---
title: Jenkins Gitlab
date: 2023-03-06 17:52:31
tags: 
- Linux
- Jenkins
- Gitlab
---

## Docker 安装 Jenkins

```bash
docker run \
-d \
-uroot \
-p 9090:8080 \
-p 50000:50000 \
--name jenkins \
-v ~/.jenkins_home:/var/jenkins_home \
-v /etc/localtime:/etc/localtime \
jenkins/jenkins
```

## Docker 安装 Gitlab

```bash
sudo docker run --detach \
  --hostname gitlab.quanwei.vip \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume ~/.gitlab/config:/etc/gitlab \
  --volume ~/.gitlab/logs:/var/log/gitlab \
  --volume ~/.gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```

