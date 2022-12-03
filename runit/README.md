# Void Linux Docker + runit

## Non-interactive example
See the [test](test) directory.

## Interactive example
```
# -- on the host --

$ git clone https://github.com/thetredev/void-docker-init.git
$ cd void-docker-init
$ docker build -t void-docker-init:runit-base runit/image
$ docker run --rm -p 8080:80 -it void-docker-init:runit-base
```

```
# -- inside docker container shell --

# install required tools for nginx (groupadd, useradd)
$ xbps-install -Sy shadow

# install nginx
$ xbps-install -Sy nginx

# run nginx as a runit service!
$ ln -s /etc/sv/nginx /var/service
$ sv status nginx
run: nginx: (pid 89) 2s
```

Head to http://localhost:8080 and check that NGINX is running. Now you should see some logs:
```
# -- inside docker container shell --

$ cat /var/log/nginx/access.log
172.17.0.1 - - [03/Dec/2022:01:35:06 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:107.0) Gecko/20100101 Firefox/107.0"
172.17.0.1 - - [03/Dec/2022:01:35:06 +0000] "GET /favicon.ico HTTP/1.1" 404 153 "http://localhost:8080/" "Mozilla/5.0 (X11; Linux x86_64; rv:107.0) Gecko/20100101 Firefox/107.0"
```
