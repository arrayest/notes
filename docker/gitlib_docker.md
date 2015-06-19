### Gitlab:
- docker pull sameersbn/gitlab:7.11.4-1

### Postgresql:
- docker pull sameersbn/postgresql:9.4

>docker run --name=postgresql-gitlab -d \
  --env='DB_NAME=gitlabhq_production' \
  --env='DB_USER=gitlab' --env='DB_PASS=password' \
  --volume=/data/gitlab/postgresql/data/:/var/lib/postgresql \
  sameersbn/postgresql:9.4

### Gitlab:
>docker run --name=gitlab -d --link=postgresql-gitlab:postgresql \
  --link=redis-gitlab:redisio \
  --volume=/data/gitlab/data/:/home/git/data \
  -p 10022:22 -p 10080:80 \
  -e VIRTUAL_HOST=gitlab.coomo99.com \
  -e GITLAB_HOST=gitlab.coomo99.com \
  -e GITLAB_EMAIL=admin@coomo99.com \
  -e SMTP_DOMAIN=qq.com \
  -e SMTP_HOST=smtp.exmail.qq.com \
  -e SMTP_PORT=587 \
  -e SMTP_USER=admin@coomo99.com \
  -e SMTP_PASS=xxxxxx \
  -e SMTP_STARTTLS=true \
  -e SMTP_OPENSSL_VERIFY_MODE=none \
  -e SMTP_AUTHENTICATION=login \
  -e GITLAB_EMAIL_DISPLAY_NAME="Coomo Gitlab" \
  -e GITLAB_EMAIL_REPLY_TO=admin@coomo99.com \
  -e GITLAB_TIMEOUT=60 \
  -e GITLAB_BACKUPS=weekly \
  -e GITLAB_SSH_PORT=10022 \
  sameersbn/gitlab:7.11.4-1

### Redis:
- docker pull sameersbn/redis:latest

- docker run --name=redis-gitlab -d sameersbn/redis:latest

- docker run --name=gitlab -d --link=redis-gitlab:redisio \
  sameersbn/gitlab:7.11.4-1

### Mail

### SSL

### Nginx-Proxy:
- docker run -e VIRTUAL_HOST=gitlab.coomo99.com

- docker run -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy


###  Gitlab mail test:

>docker exec -it gitlab "/bin/bash"
RAILS_ENV=production bin/rails c
Notify.test_email("546411936@qq.com", "hi","hello world")
